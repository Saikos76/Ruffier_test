# Ruffier_test
from kivy.app import App 
from kivy.uix.screenmanager import ScreenManager, Screen 
from kivy.uix.boxlayout import BoxLayout 
from kivy.uix.label import Label 
from kivy.uix.textinput import TextInput 
from kivy.uix.button import Button 
from instructions import * 
from ruffier import *
from seconds import *
from kivy.animation import Animation 
 
# Global variables (name, age, 3 types of pulse and heart level)
user_name = "" 
user_age = 0 
pulse1 = 0   
pulse2 = 0 
pulse3 = 0 
level = 0   
 
class InputScreen(Screen): 
    def __init__(self, **kwargs): 
        super(InputScreen, self).__init__(**kwargs) 
        self.layout = BoxLayout(orientation="vertical") 
 
        instruction_label = Label(text=txt_instruction, size_hint_y=None, height=100) 
        name_input = TextInput(hint_text="Введите имя", size_hint_y=None, height=50) 
        age_input = TextInput(hint_text="Введите возраст", size_hint_y=None, height=50) 
        button_start = Button(text="Начать тест", size_hint_y=None, height=50) 
        button_start.bind(on_release=lambda instance: self.start_test(name_input.text, age_input.text)) 
 
        self.layout.add_widget(instruction_label) 
        self.layout.add_widget(name_input) 
        self.layout.add_widget(age_input) 
        self.layout.add_widget(button_start) 
 
        self.add_widget(self.layout) 
 
    def start_test(self, name, age): 
        global user_name, user_age 
        user_name = name 
        try: 
            user_age = int(age) 
            self.manager.current = 'pulse'  
        except ValueError: 
            pass 
 
class PulseScreen(Screen): 
    def __init__(self,name='pulse', **kwargs): 
        super(PulseScreen, self).__init__(name=name,**kwargs) 
        self.layout = BoxLayout(orientation="vertical") 
        pulse_label = Label(text=txt_test1, size_hint_y=None, height=100) 
        pulse_input = TextInput(hint_text="Результат пульса", size_hint_y=None, height=50) 
        continue_button = Button(text="Продолжить", size_hint_y=None, height=50) 
        continue_button.bind(on_release=lambda instance: self.save_pulse_and_continue(pulse_input.text)) 
 
        self.layout.add_widget(pulse_label) 
        self.layout.add_widget(pulse_input) 
        self.layout.add_widget(continue_button) 
        self.add_widget(self.layout) 

"""        self.lbl_sec = Seconds(15)
        self.lbl.sec.bind(done=self.sec_finished)
    def sec_finished(self, *args):
        self.in_result.set_disabled(False)
        self.btn.set_disabled(False)
        self.btn.text = 'Продолжить'
    def next(self):
        if not self.next_screen:
            self.btn.set_disabled(True)
            self.lbl_sec.start()
        else:
            self.manager.current = 'sits'"""
"""class PulseScreen2(Screen):
    def __init__(self, **kwargs)
 
    def save_pulse_and_continue(self, pulse_value_1): 
        global pulse1 
        pulse1 = pulse_value_1   
        self.manager.current = 'sits'"""
 
class SitsScreen(Screen): 
    def __init__(self, name = 'sits', **kwargs): 
        super(SitsScreen, self).__init__(name=name, **kwargs) 
        self.layout = BoxLayout(orientation="vertical") 
 
        instruction_label = Label(text=txt_test2, size_hint_y=None, height=100) 
 
        continue_button = Button(text="Начать", size_hint_y=None, height=50, background_normal='', background_color=(0, 1, 0, 1)) 
        continue_button.bind(on_release=self.continue_to_next) 
 
        self.layout.add_widget(instruction_label) 
        self.layout.add_widget(continue_button) 
        self.add_widget(self.layout) 
 
    def continue_to_next(self, instance): 
        self.manager.current = 'result' 
 
class ResultScreen(Screen): 
    def __init__(self, name = 'result', **kwargs): 
        super(ResultScreen, self).__init__(name=name, **kwargs) 
        self.layout = BoxLayout(orientation="vertical") 
 
        instruction_label = Label( 
            text=txt_test3, 
            size_hint_y=None, height=100 
        ) 
 
        pulse_input_1 = TextInput(hint_text="За первые 15 секунд минуты", size_hint_y=None, height=50) 
        pulse_input_2 = TextInput(hint_text="За последние 15 секунд минуты", size_hint_y=None, height=50) 
 
        # button "завершить" - ends the test
        finish_button = Button(text="Завершить", size_hint_y=None, height=50, background_normal='', background_color=(0, 1, 0, 1)) 
        finish_button.bind(on_release=lambda instance: self.save_pulses_and_continue(pulse_input_1.text, pulse_input_2.text)) 
 
        self.layout.add_widget(instruction_label) 
        self.layout.add_widget(pulse_input_1)


class HeartCheckApp(App):
    def build(self):
        sm = ScreenManager()
        sm.add_widget(InputScreen())
        sm.add_widget(PulseScreen())
        sm.add_widget(SitsScreen())
        sm.add_widget(ResultScreen())
        return sm

app = HeartCheckApp()
app.run() 
