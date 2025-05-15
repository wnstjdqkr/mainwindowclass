# mainWinClass
## 소스코드 설명 

1. mainWindow 만들기   
2. Class 단위로 만들기   
3. 몇개의 Class로 만들어진 APP   
   
### pyQTapp01.py
```
from PyQt6 import uic
from PyQt6.QtWidgets import QApplication

Form, Window = uic.loadUiType("dialog.ui")

app = QApplication([])
window = Window()
form = Form()
form.setupUi(window)
window.show()
app.exec()
```   
pyQTapp02와 pyQTapp03은 기능을 확장한것들이    

### pyQTapp04.py는 클래스로 만든 것이다.     
```
from PyQt6.QtGui import QPixmap
from PyQt6.QtWidgets import QFileDialog, QApplication
from PyQt6 import uic
import csv

Form, Window = uic.loadUiType("res/fbDialog.ui")

class myWindow:
    def __init__(self):
        self.window = Window()
        self.form = Form()
        self.form.setupUi(self.window)

        # Connect signals to slots
        self.form.btnSubmit.clicked.connect(self.save_to_csv)
        self.form.btnSearch.clicked.connect(self.fine_from_csv)
        self.form.btnAddPhoto.clicked.connect(self.add_photo)

    def save_to_csv(self):
        text1 = self.form.lineEdit.text() if self.form.lineEdit.text() else ""
        text2 = self.form.lineEdit_2.text() if self.form.lineEdit_2.text() else ""
        text3 = self.form.lineEdit_3.text() if self.form.lineEdit_3.text() else ""
        with open("add.txt", "a", newline="", encoding="utf-8") as file:
            writer = csv.writer(file)
            writer.writerow([text1, text2, text3])

    def fine_from_csv(self):
        self.form.lineEdit.clear()
        self.form.lineEdit_2.clear()
        self.form.lineEdit_3.clear()
        with open("add.txt", "r", encoding="utf-8") as file:
            reader = csv.reader(file)
            for row in reader:
                if row:
                    self.form.lineEdit.setText(row[0])
                    self.form.lineEdit_2.setText(row[1])
                    self.form.lineEdit_3.setText(row[2])
                    break

    def add_photo(self):
        file_path, _ = QFileDialog.getOpenFileName(
            None, "Select Image", "", "Image Files (*.png *.jpg *.jpeg *.bmp *.gif)"
        )
        if file_path:
            pixmap = QPixmap(file_path)
            self.form.lblPhoto.setPixmap(pixmap)
            self.form.lblPhoto.setScaledContents(True)
            self.form.lineEdit_3.setText(file_path)
    def lounch(self):
        self.window.show()
if __name__ == "__main__":
    app = QApplication([])
    mw = myWindow()
    mw.window.show()
    app.exec()
```
### 그리고 최종적으로 app.py를 만들었다    
```
from PyQt6 import uic
from PyQt6.QtWidgets import QApplication
from PyQt6.QtCore import QUrl
from PyQt6.QtGui import QDesktopServices
from pyQTapp04 import myWindow

Form, Window = uic.loadUiType("res/mainWindow.ui")

class MainWindow:
    def __init__(self):
        self.window = Window()
        self.form = Form()
        self.form.setupUi(self.window)
        self.fb_window = myWindow()  # myWindow 인스턴스 생성

        # 메뉴 액션 연결
        self.form.actionMini_FaceBook.triggered.connect(self.fb_window.lounch)
        self.form.actionITEDU.triggered.connect(lambda: QDesktopServices.openUrl(QUrl("https://itedu.wsu.ac.kr")))
        self.form.actionWSU.triggered.connect(lambda: QDesktopServices.openUrl(QUrl("https://www.wsu.ac.kr")))

    def lounch(self):
        self.window.show()

if __name__ == "__main__":
    app = QApplication([])
    mw = MainWindow()
    mw.lounch()
    app.exec()

```

이것은 mainWindow로 맨처름 실행 되는 것이다. 여기서 메뉴를 클릭했을때    
위의 pyQTapp04.py가 실행 되는 것이다.    
<img width="429" alt="image" src="https://github.com/user-attachments/assets/3eb7a4a1-61cc-4746-9556-2cdefeb2c12a" />

프로그램은 위와 같이 구성 되어 있다     

class,를 사용하는것은 객체를 하나의 최상위 class name으로 관리 하기 위함이다.    
또한 모듈로 완성된 py 코드는 재활용이 가능하다.
