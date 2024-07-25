# Creating PySide6 Executable File With Translations
### Starting
Install ```pyinstaller``` to your python environment as follows:
```bash
pip install pyinstaller
```
### Step #1
To create an .exe file with translations, use the following command to create a ```.spec``` file for the code:
```bash
pyi-makespec --onefile <filename>.py
```

### Step #2
After creating the ```.spec``` file, it is needed to be edited accordingly to find translation files (The file below is an example, change it for the needs):

**For only translation files**
```python
# -*- mode: python ; coding: utf-8 -*-

# getting path of the translations folder and files in it
def get_translations_data():
    translations_data = []
    for locale in os.listdir(os.path.join('./translations')):
        translations_data.append((
            os.path.join('./translations', locale, 'LC_MESSAGES/*.mo'),
            os.path.join('translations', locale, 'LC_MESSAGES')
        ))
    return translations_data

a = Analysis(
    ['main.py'],
    pathex=[],
    binaries=[],
    datas=get_translations_data(),          # importing the translation files
    hiddenimports=[],
    hookspath=[],
    hooksconfig={},
    runtime_hooks=[],
    excludes=[],
    noarchive=False,
    optimize=0,
)
pyz = PYZ(a.pure)

exe = EXE(
    pyz,
    a.scripts,
    a.binaries,
    a.datas,
    [],
    name='main',
    debug=False,
    bootloader_ignore_signals=False,
    strip=False,
    upx=True,
    upx_exclude=[],
    runtime_tmpdir=None,
    console=False,                      # hiding the console while ui pops up
    disable_windowed_traceback=False,
    argv_emulation=False,
    target_arch=None,
    codesign_identity=None,
    entitlements_file=None,
)
```


**For translation files + fonts**
```python
# -*- mode: python ; coding: utf-8 -*-

# getting path of the translations folder and files in it
def get_translations_data():
    translations_data = []
    for locale in os.listdir(os.path.join('./translations')):
        translations_data.append((
            os.path.join('./translations', locale, 'LC_MESSAGES/*.mo'),
            os.path.join('translations', locale, 'LC_MESSAGES')
        ))
    return translations_data

def get_fonts_data():
    fonts_data = []
    font_dir = './font/tinos' # adjust this part as needed
    for font_file in os.listdir(font_dir):
        if font_file.endswith('.ttf'):
            fonts_data.append((os.path.join(font_dir, font_file), 'font/tinos')) # adjust this part as needed
    return fonts_data

def get_all_data():
    return get_translations_data() + get_fonts_data()
a = Analysis(
    ['main.py'],
    pathex=[],
    binaries=[],
    datas=get_all_data(),          # importing the translation files
    hiddenimports=[],
    hookspath=[],
    hooksconfig={},
    runtime_hooks=[],
    excludes=[],
    noarchive=False,
    optimize=0,
)
pyz = PYZ(a.pure)

exe = EXE(
    pyz,
    a.scripts,
    a.binaries,
    a.datas,
    [],
    name='main',
    debug=False,
    bootloader_ignore_signals=False,
    strip=False,
    upx=True,
    upx_exclude=[],
    runtime_tmpdir=None,
    console=False,                      # hiding the console while ui pops up
    disable_windowed_traceback=False,
    argv_emulation=False,
    target_arch=None,
    codesign_identity=None,
    entitlements_file=None,
)
```

### Step #3
After editing the ```.spec``` file, run the following code to generate an ```.exe``` file:
```bash
pyinstaller <filename>.spec
```