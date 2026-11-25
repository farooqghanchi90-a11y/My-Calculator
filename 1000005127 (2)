
import ast
import math

from kivy.app import App
from kivy.metrics import dp, sp
from kivy.core.window import Window
from kivy.properties import ListProperty
from kivy.graphics import Color, RoundedRectangle
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.gridlayout import GridLayout
from kivy.uix.button import Button
from kivy.uix.label import Label

Window.clearcolor = (0.96, 0.94, 1, 1)


# Rounded pastel button
class CuteButton(Button):

    bg_color = ListProperty([0.85, 0.80, 0.96, 1])

    def __init__(self, **kwargs):
        super().__init__(**kwargs)

        self.background_normal = ""
        self.background_down = ""
        self.background_color = (0, 0, 0, 0)

        with self.canvas.before:
            self.shape_color = Color(*self.bg_color)
            self.shape = RoundedRectangle(
                pos=self.pos,
                size=self.size,
                radius=[dp(17)]
            )

        self.bind(pos=self.update_shape,
                  size=self.update_shape,
                  bg_color=self.update_color)

    def update_shape(self, *args):
        self.shape.pos = self.pos
        self.shape.size = self.size

    def update_color(self, *args):
        self.shape_color.rgba = self.bg_color


# Rounded display panel
class CutePanel(BoxLayout):

    bg_color = ListProperty([1, 1, 1, 1])

    def __init__(self, **kwargs):
        super().__init__(**kwargs)

        with self.canvas.before:
            self.panel_color = Color(*self.bg_color)
            self.panel_shape = RoundedRectangle(
                pos=self.pos,
                size=self.size,
                radius=[dp(22)]
            )

        self.bind(pos=self.update_panel,
                  size=self.update_panel,
                  bg_color=self.update_panel_color)

    def update_panel(self, *args):
        self.panel_shape.pos = self.pos
        self.panel_shape.size = self.size

    def update_panel_color(self, *args):
        self.panel_color.rgba = self.bg_color


class DreamCalculator(App):

    def build(self):

        self.expression = ""
        self.angle_mode = "DEG"
        self.history = []

        main = BoxLayout(
            orientation="vertical",
            padding=dp(12),
            spacing=dp(9)
        )

        # Header
        header = Label(
            text="♡ Scientific Calculator ♡",
            font_size=sp(25),
            bold=True,
            color=(0.35, 0.26, 0.55, 1),
            size_hint_y=0.08
        )

        main.add_widget(header)

        subtitle = Label(
            text="Dream • Calculate • Achieve ✨",
            font_size=sp(13),
            color=(0.55, 0.43, 0.68, 1),
            size_hint_y=0.04
        )

        main.add_widget(subtitle)

        # Display
        display_panel = CutePanel(
            orientation="vertical",
            padding=dp(14),
            spacing=dp(4),
            bg_color=(0.91, 0.87, 0.98, 1),
            size_hint_y=0.17
        )

        self.expression_label = Label(
            text="DEG",
            font_size=sp(20),
            color=(0.40, 0.32, 0.56, 1),
            halign="right",
            valign="middle",
            size_hint_y=0.4
        )

        self.expression_label.bind(
            size=lambda obj, value:
            setattr(obj, "text_size", value)
        )

        self.result_label = Label(
            text="0",
            font_size=sp(36),
            bold=True,
            color=(0.20, 0.15, 0.35, 1),
            halign="right",
            valign="middle",
            size_hint_y=0.6
        )

        self.result_label.bind(
            size=lambda obj, value:
            setattr(obj, "text_size", value)
        )

        display_panel.add_widget(self.expression_label)
        display_panel.add_widget(self.result_label)

        main.add_widget(display_panel)

        # Mode and history
        top = BoxLayout(
            size_hint_y=0.075,
            spacing=dp(7)
        )

        self.mode_button = CuteButton(
            text="DEG / RAD",
            font_size=sp(16),
            bold=True,
            bg_color=(0.80, 0.72, 0.94, 1)
        )

        self.mode_button.bind(
            on_press=self.change_mode
        )

        history_button = CuteButton(
            text="♡ History",
            font_size=sp(16),
            bg_color=(0.91, 0.83, 0.96, 1)
        )

        history_button.bind(
            on_press=self.show_history
        )

        top.add_widget(self.mode_button)
        top.add_widget(history_button)

        main.add_widget(top)

        # Keypad
        buttons = [
            ["sin", "cos", "tan", "log"],
            ["ln", "√", "π", "e"],
            ["x²", "x³", "1/x", "!"],
            ["(", ")", "^", "DEL"],
            ["7", "8", "9", "÷"],
            ["4", "5", "6", "×"],
            ["1", "2", "3", "−"],
            ["0", ".", "%", "+"],
            ["AC", "Ans", "=", "±"]
        ]

        grid = GridLayout(
            cols=4,
            spacing=dp(7),
            size_hint_y=0.61
        )

        for row in buttons:

            for item in row:

                color = self.get_button_color(item)

                button = CuteButton(
                    text=item,
                    font_size=sp(18),
                    bold=True,
                    bg_color=color,
                    color=(0.25, 0.18, 0.38, 1)
                )

                button.bind(
                    on_press=lambda instance, value=item:
                    self.press(value)
                )

                grid.add_widget(button)

        main.add_widget(grid)

        # Footer
        self.footer = Label(
            text="Made with ♡",
            font_size=sp(13),
            color=(0.55, 0.42, 0.65, 1),
            size_hint_y=0.045
        )

        main.add_widget(self.footer)

        return main

    def get_button_color(self, item):

        if item == "=":
            return (0.94, 0.53, 0.77, 1)

        if item in ["AC", "DEL"]:
            return (0.97, 0.70, 0.81, 1)

        if item in [
            "sin", "cos", "tan", "log",
            "ln", "√", "π", "e",
            "x²", "x³", "1/x", "!"
        ]:
            return (0.81, 0.74, 0.95, 1)

        if item in ["+", "−", "×", "÷", "^", "%", "±"]:
            return (0.87, 0.80, 0.96, 1)

        return (1, 0.97, 1, 1)

    def change_mode(self, instance):

        if self.angle_mode == "DEG":
            self.angle_mode = "RAD"
        else:
            self.angle_mode = "DEG"

        self.mode_button.text = self.angle_mode
        self.expression_label.text = self.angle_mode

    def evaluate(self, expression):

        def trig(fn, x):
            if self.angle_mode == "DEG":
                x = math.radians(x)
            return fn(x)

        def factorial(x):
            if x < 0 or not float(x).is_integer():
                raise ValueError("Invalid factorial")
            if x > 170:
                raise ValueError("Too large")
            return math.factorial(int(x))

        functions = {
            "sin": lambda x: trig(math.sin, x),
            "cos": lambda x: trig(math.cos, x),
            "tan": lambda x: trig(math.tan, x),
            "sqrt": math.sqrt,
            "log": math.log10,
            "ln": math.log,
            "fact": factorial,
            "abs": abs
        }

        constants = {
            "pi": math.pi,
            "e": math.e
        }

        def calculate(node):

            if isinstance(node, ast.Constant):
                if type(node.value) in (int, float):
                    return node.value
                raise ValueError("Invalid number")

            if isinstance(node, ast.Name):
                if node.id in constants:
                    return constants[node.id]
                raise ValueError("Invalid name")

            if isinstance(node, ast.BinOp):

                a = calculate(node.left)
                b = calculate(node.right)

                if isinstance(node.op, ast.Add):
                    return a + b

                if isinstance(node.op, ast.Sub):
                    return a - b

                if isinstance(node.op, ast.Mult):
                    return a * b

                if isinstance(node.op, ast.Div):
                    return a / b

                if isinstance(node.op, ast.Pow):
                    if abs(b) > 1000:
                        raise ValueError("Power too large")
                    return a ** b

                if isinstance(node.op, ast.Mod):
                    return a % b

            if isinstance(node, ast.UnaryOp):

                value = calculate(node.operand)

                if isinstance(node.op, ast.USub):
                    return -value

                if isinstance(node.op, ast.UAdd):
                    return value

            if isinstance(node, ast.Call):

                if not isinstance(node.func, ast.Name):
                    raise ValueError("Invalid function")

                name = node.func.id

                if name not in functions or len(node.args) != 1:
                    raise ValueError("Invalid function")

                return functions[name](
                    calculate(node.args[0])
                )

            raise ValueError("Invalid expression")

        tree = ast.parse(expression, mode="eval")
        result = calculate(tree.body)

        if isinstance(result, complex):
            raise ValueError("Invalid result")

        if not math.isfinite(result):
            raise ValueError("Invalid result")

        return result

    def update_display(self):

        self.expression_label.text = (
            self.angle_mode + "  |  " +
            (self.expression or "Ready ♡")
        )

        self.result_label.text = self.expression or "0"

    def press(self, item):

        if item == "AC":

            self.expression = ""
            self.result_label.text = "0"
            self.expression_label.text = self.angle_mode
            return

        if item == "DEL":

            self.expression = self.expression[:-1]
            self.update_display()
            return

        if item == "Ans":

            if self.history:
                answer = self.history[-1][1]
                self.expression += str(answer)
                self.update_display()
            return

        if item == "±":

            if self.expression:
                self.expression = "-(" + self.expression + ")"
                self.update_display()
            return

        if item == "=":

            try:
                old = self.expression
                result = self.evaluate(old)

                self.history.append((old, result))

                self.expression = str(result)

                self.expression_label.text = old
                self.result_label.text = str(result)

                self.footer.text = "♡ Calculation completed ♡"

            except Exception:
                self.result_label.text = "Error"
                self.footer.text = "Check your expression ♡"
                self.expression = ""

            return

        if item == "History":
            self.show_history(None)
            return

        if self.expression == "":
            pass

        functions = [
            "sin", "cos", "tan",
            "log", "ln"
        ]

        if item in functions:
            self.expression += item + "("

        elif item == "√":
            self.expression += "sqrt("

        elif item == "π":
            self.expression += "pi"

        elif item == "e":
            self.expression += "e"

        elif item == "x²":
            self.expression += "**2"

        elif item == "x³":
            self.expression += "**3"

        elif item == "1/x":
            self.expression += "1/("

        elif item == "!":
            self.expression += "fact("

        else:

            replacements = {
                "×": "*",
                "÷": "/",
                "−": "-"
            }

            self.expression += replacements.get(item, item)

        self.update_display()

    def show_history(self, instance):

        if not self.history:
            self.expression_label.text = "History"
            self.result_label.text = "No calculations yet"
            return

        lines = [
            str(a) + " = " + str(b)
            for a, b in self.history[-3:]
        ]

        self.expression_label.text = "♡ Recent calculations"
        self.result_label.text = "\n".join(lines)


DreamCalculator().run()
