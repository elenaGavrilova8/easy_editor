# easy_editor
New project
from PyQt5.QtCore import Qt
from PyQt5.QtWidgets import QApplication, QWidget, QPushButton, QLabel, QHBoxLayout, QVBoxLayout, QListWidget, QFileDialog
from PyQt5.QtGui import QPixmap
import os 
from PIL import Image
from PIL.ImageFilter import SHARPEN
app = QApplication([])

class Imagerofessor():
    def __init__(self):
        self.image = None
        self.filename = None
        self.papkaname = 'Mode/'
        
    def loadImage(self, filename):
        self.filename = filename
        image_path = os.path.join(workdir, filename)
        self.image = Image.open(image_path)

    def showImage(self, path):
        picture.hide()
        pixmapimage = QPixmap(path)
        w, h = picture.width(), picture.height()
        pixmapimage = pixmapimage.scaled(w, h, Qt.KeepAspectRatio)
        picture.setPixmap(pixmapimage)
        picture.show()

    def do_bw(self):
        self.image = self.image.convert('L')
        self.saveImage()
        image_path = os.path.join(workdir, self.papkaname, self.filename)
        self.showImage(image_path)

    def saveImage(self):
        path = os.path.join(workdir, self.papkaname)
        if not(os.path.exists(path) or os.path.isdir(path)):
            os.mkdir(path)
        image_path = os.path.join(path, self.filename)
        self.image.save(image_path)
    def do_right(self):
        self.image = self.image.transpose(Image.ROTATE_270)
        self.saveImage()
        image_path = os.path.join(workdir, self.papkaname, self.filename)
        self.showImage(image_path)

    def do_left(self):
        self.image = self.image.transpose(Image.ROTATE_90)
        self.saveImage()
        image_path = os.path.join(workdir, self.papkaname, self.filename)
        self.showImage(image_path)

    def do_mirrow(self):
        self.image = self.image.transpose(Image.FLIP_LEFT_RIGHT)
        self.saveImage()
        image_path = os.path.join(workdir, self.papkaname, self.filename)
        self.showImage(image_path)

    def do_bright(self):
        self.image = self.image.filter(SHARPEN)
        self.saveImage()
        image_path = os.path.join(workdir, self.papkaname, self.filename)
        self.showImage(image_path)


    
def showChosenImage():
    if list_pic.currentRow()>= 0:
        filename = list_pic.currentItem().text()
        workimage.loadImage(filename)
        image_path = os.path.join(workdir, workimage.filename)
        workimage.showImage(image_path)


m_win = QWidget()
m_win.setWindowTitle('Easy Editor')
m_win.resize(900, 700)

picture = QLabel('Картинка')
papka = QPushButton('Папка')
list_pic = QListWidget()
button1 = QPushButton('Лево')
button2 = QPushButton('Право')
button3 = QPushButton('Зеркало')
button4 = QPushButton('Резкость')
button5 = QPushButton('Ч/Б')

medi_line1 = QVBoxLayout()
medi_line1.addWidget(papka, stretch= 3 )
medi_line1.addWidget(list_pic, stretch= 3)
#medi_line1.addStretch(1)

medi_line2 = QVBoxLayout()
medi_line2.addWidget(picture)
min_lin = QHBoxLayout()
min_lin.addWidget(button1)
min_lin.addWidget(button2)
min_lin.addWidget(button3)
min_lin.addWidget(button4)
min_lin.addWidget(button5)
medi_line2.addLayout(min_lin)


main_lin = QHBoxLayout()
main_lin.addLayout(medi_line1,15)
main_lin.addLayout(medi_line2,50)

def chooseWorkdir():
    global workdir
    workdir = QFileDialog.getExistingDirectory()

def filter(files, extensions):
    result = []
    for filel in files:
        for extension in extensions:
            if filel.endswith(extension):
                result.append(filel)
    return result

def showFilenamesList():
    chooseWorkdir()
    ok = ['.png', '.jpg', '.jpeg', '.bmp']
    files = os.listdir(workdir)
    filenames = filter(files, ok)
    list_pic.clear()
    for i in filenames:
        list_pic.addItem(i)


workimage = Imagerofessor()

         
papka.clicked.connect(showFilenamesList)
m_win.setLayout(main_lin)
button5.clicked.connect(workimage.do_bw)
button2.clicked.connect(workimage.do_right)
button1.clicked.connect(workimage.do_left)
button3.clicked.connect(workimage.do_mirrow)
button4.clicked.connect(workimage.do_bright)



list_pic.currentRowChanged.connect(showChosenImage)

m_win.show()
app.exec_()
