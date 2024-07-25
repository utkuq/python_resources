# How to Translate Python Applications
### Starting 
Get xgettext from [this](https://www.gnu.org/software/gettext/) website<br>
For windows, directly download from [here](https://mlocati.github.io/articles/gettext-iconv-windows.html)

### Step #1
To use gettext inside of a python project, create a starting code snippet like below:
```Python
import gettext
def get_base_path():
    if getattr(sys, 'frozen', False):
        # Running in a PyInstaller bundle
        return sys._MEIPASS
    else:
        # Running in a development environment
        return os.path.dirname(os.path.abspath(__file__))

# Get the base path
base_path = get_base_path()

# Construct the path to the translations directory
localedir = os.path.join(base_path, 'translations')


currentLanguage = "English"
english = gettext.translation(
    'messages', localedir=localedir, languages=['en'])
turkish = gettext.translation(
    'messages', localedir=localedir, languages=['tr'])
english.install()
_ = gettext.gettext
```
It is necessary to initially define each language that can be used throughout the application.
After defining ```_``` character as an object from ```gettext``` module, wrap all the strings with ```_()``` variable and type each string inside of the braces as follows: 
```_("Some String Value")```
### Step #2
Create a directory under root directory of the project named as ```translations```
### Step #3
Create sub-directories under ```translations``` with target translation languages such as ```en``` for English and ```tr``` for Turkish.
### Step #4
Create sub-directories under target languages' folders named as ```LC_MESSAGES```
So the final folder tree should be like this:
```bash
├───translations
│   ├───en
│   │   └───LC_MESSAGES
│   └───tr
│       └───LC_MESSAGES
```
### Step #5
Get strings from specified file in desired languages:

**For English:**
```bash
xgettext --language=Python --keyword=_ --output=translations/en/LC_MESSAGES/messages.po <Your-File-Name>.py
```

**For Turkish:**
```bash
xgettext --language=Python --keyword=_ --output=translations/tr/LC_MESSAGES/messages.po <Your-File-Name>.py
```
<br>

If there are multiple ```.py``` files needs to be translated simultaneously, the command above can be modified as follows:


**For English:**
```bash
xgettext --language=Python --keyword=_ --output=translations/en/LC_MESSAGES/messages.po <file_1>.py <file_2>.py
```

**For Turkish:**
```bash
xgettext --language=Python --keyword=_ --output=translations/tr/LC_MESSAGES/messages.po <file_1>.py <file_2>.py
```

### Step #6
After translating the strings located at ```messages.po``` file from target language's folder, run the following commands:

**For English:**
```bash
msgfmt -o translations/en/LC_MESSAGES/messages.mo translations/en/LC_MESSAGES/messages.po
```

**For Turkish:**
```bash
msgfmt -o translations/tr/LC_MESSAGES/messages.mo translations/tr/LC_MESSAGES/messages.po
```

### Step #7
To switch between languages use the code snippet below:
```python
global english, turkish, _, currentLanguage
if action.text() == "English":
    english.install()
    _ = english.gettext
    currentLanguage = "English"
elif action.text() == "Türkçe":
    turkish.install()
    _ = turkish.gettext
    currentLanguage = "Türkçe"
```
If the user interface needs to be updated with selected language, it is necessary to create a function named ```retranslateUi``` to reload components from the application. For PySide6, it can be done as follows:
```python
def retranslateUi(self):
    # for labels
    self.my_label.setText(_("Same label text"))

    # for buttons
    self.my_button.setText(_("Same button text"))

    # for comboboxes
    items = [_("First item"), _("Second item"), _("Third item")] # they are orignally written same as before
    self.my_combobox.clear()
    self.my_combobox.addItems(items)
    self.my_combobox.setCurrentIndex(-1) # sets combobox as unselected when the ui is loaded

```
An alternative way (basically same method with less confusion) to update comboboxes:
```python
def update_combobox_items(self, widget, items):
        widget.clear()
        widget.addItems(items)
        widget.setCurrentIndex(-1)

def retranslateUi(self):
    # for labels
    self.my_label.setText(_("Same label text"))

    # for buttons
    self.my_button.setText(_("Same button text"))

    # for comboboxes
    self.update_combobox_items(self.my_combobox, [_("First item"), _("Second item"), _("Third item")]) # the items are orignally written same as before.
```