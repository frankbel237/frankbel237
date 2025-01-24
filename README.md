from kivy.app import App
from kivy.uix.screenmanager import ScreenManager, Screen
from kivy.lang import Builder
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.label import Label
from kivy.uix.button import Button
from kivy.uix.textinput import TextInput
from kivymd.uix.dialog import MDDialog
from kivymd.uix.button import MDRectangleFlatButton

# Define the app screens
class LoginScreen(Screen):
    def _init_(self, **kwargs):
        super()._init_(**kwargs)
        layout = BoxLayout(orientation='vertical', padding=20, spacing=10)

        self.username = TextInput(hint_text="Username", size_hint=(1, None), height=40)
        self.password = TextInput(hint_text="Password", password=True, size_hint=(1, None), height=40)
        login_button = Button(text="Login", size_hint=(1, None), height=40)
        login_button.bind(on_press=self.login)

        layout.add_widget(Label(text="ATAHmail Login", font_size=24))
        layout.add_widget(self.username)
        layout.add_widget(self.password)
        layout.add_widget(login_button)

        self.add_widget(layout)

    def login(self, instance):
        if self.username.text == "user" and self.password.text == "password":
            self.manager.current = "dashboard"
        else:
            self.show_error_dialog("Invalid username or password")

    def show_error_dialog(self, message):
        dialog = MDDialog(
            title="Error",
            text=message,
            buttons=[MDRectangleFlatButton(text="OK", on_release=lambda x: dialog.dismiss())]
        )
        dialog.open()


class DashboardScreen(Screen):
    def _init_(self, **kwargs):
        super()._init_(**kwargs)
        layout = BoxLayout(orientation='vertical', padding=20, spacing=10)

        welcome_label = Label(text="Welcome to ATAHmail!", font_size=24)
        track_button = Button(text="Track Package", size_hint=(1, None), height=40)
        track_button.bind(on_press=self.track_package)

        layout.add_widget(welcome_label)
        layout.add_widget(track_button)

        self.add_widget(layout)

    def track_package(self, instance):
        self.manager.current = "tracking"


class TrackingScreen(Screen):
    def _init_(self, **kwargs):
        super()._init_(**kwargs)
        layout = BoxLayout(orientation='vertical', padding=20, spacing=10)

        self.tracking_id = TextInput(hint_text="Enter Tracking ID", size_hint=(1, None), height=40)
        track_button = Button(text="Track", size_hint=(1, None), height=40)
        track_button.bind(on_press=self.track)

        layout.add_widget(Label(text="Track Your Package", font_size=24))
        layout.add_widget(self.tracking_id)
        layout.add_widget(track_button)

        self.add_widget(layout)

    def track(self, instance):
        tracking_id = self.tracking_id.text
        if tracking_id:
            self.show_tracking_info(f"Package {tracking_id} is out for delivery.")
        else:
            self.show_error_dialog("Please enter a valid tracking ID.")

    def show_tracking_info(self, message):
        dialog = MDDialog(
            title="Tracking Info",
            text=message,
            buttons=[MDRectangleFlatButton(text="OK", on_release=lambda x: dialog.dismiss())]
        )
        dialog.open()

    def show_error_dialog(self, message):
        dialog = MDDialog(
            title="Error",
            text=message,
            buttons=[MDRectangleFlatButton(text="OK", on_release=lambda x: dialog.dismiss())]
        )
        dialog.open()


# Screen Manager
class ATAHmailApp(App):
    def build(self):
        sm = ScreenManager()
        sm.add_widget(LoginScreen(name="login"))
        sm.add_widget(DashboardScreen(name="dashboard"))
        sm.add_widget(TrackingScreen(name="tracking"))
        return sm


# Run the app
if _name_ == "_main_":
    ATAHmailApp().run():
