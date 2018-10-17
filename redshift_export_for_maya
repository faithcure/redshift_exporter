


import maya.cmds as cm
from PySide2.QtWidgets import *
from PySide2.QtCore import *
import os
class Form(QDialog):
    def __init__(self, parent = None):
        super(Form, self).__init__(parent)
        
        self.listItem = QListWidget()
        self.listNodes = [ user for user in cm.ls(sl=True)]
        self.listItem.addItems(self.listNodes)
        self.listItem.setSelectionMode(QAbstractItemView.ExtendedSelection)
        
        self.searchBar = QLineEdit()
        self.searchBar.setPlaceholderText("Search in items")
        self.searchBar.textChanged.connect(self.filterList)
        
        self.selectFrame = QCheckBox()
        self.selectFrame.setText("Select a frame number: ")
        self.selectFrameLine = QLineEdit()
        self.selectFrameLine.setPlaceholderText("Frame")
        
        self.selectFrameLine.returnPressed.connect(self.printID)
                
        self.selectFrame.stateChanged.connect(self.selectFrameOperation)


        self.allFrames = QCheckBox()
        self.allFrames.setText("Current Time Slider (visible time range)")
        self.allFrames.stateChanged.connect(self.allFramesOperation)

        
        self.frameRange = QCheckBox()
        self.frameRange.setText("Frame range:")
        self.startFrame = QLineEdit()
        self.startFrame.setPlaceholderText("Start Frame")
        self.endFrame = QLineEdit()
        self.endFrame.setPlaceholderText("End Frame")
        self.frameRange.stateChanged.connect(self.frameRangeOperation)

        
        self.exportButton = QPushButton()
        self.exportButton.setText("Export")
        
        self.closeButton = QPushButton("Close")
        self.closeButton.clicked.connect(self.close)
        
        self.infoLabel = QLabel("")
        pathinfo = os.path.dirname(cm.file(q=1, sn=1)) + "/"+ os.path.basename(cm.file(q=1, sn=1))
        self.infoPathLabel = QLineEdit()
        
        self.infoPathLabel.setText(str(pathinfo))
        self.itemName = QLabel()
       
        self.progressBar = QProgressBar()
        self.mainProgressBar = QProgressBar()
     


        
        
        self.mainLayout = QVBoxLayout()
        
        self.checkLayout = QVBoxLayout()
        
        self.selectedFrameLayout = QHBoxLayout()
        
        self.frameRangeLayout = QHBoxLayout()
        self.frameRangeLayout.addWidget(self.frameRange)
        self.frameRangeLayout.addWidget(self.startFrame)
        self.frameRangeLayout.addWidget(self.endFrame)
        
        self.checkLayout.addWidget(self.allFrames)
        self.mainLayout.addWidget(self.listItem)
        self.mainLayout.addWidget(self.infoPathLabel)
        self.mainLayout.addWidget(self.searchBar)
        self.selectedFrameLayout.addWidget(self.selectFrame)
        self.selectedFrameLayout.addWidget(self.selectFrameLine)
        self.mainLayout.addLayout(self.selectedFrameLayout)
        
        self.mainLayout.addLayout(self.checkLayout)
        self.mainLayout.addLayout(self.frameRangeLayout)
        self.mainLayout.addWidget(self.infoLabel)
        self.mainLayout.addWidget(self.exportButton)
        self.mainLayout.addWidget(self.closeButton)

        
        self.setLayout(self.mainLayout)
    
    def selectFrameOperation(self, state):
        if state == Qt.Checked:
            self.exportButton.clicked.connect(self.exportRedshiftProxy_currentFrame)
            self.allFrames.setEnabled(False)
            self.frameRange.setEnabled(False)
            self.startFrame.setEnabled(False)
            self.endFrame.setEnabled(False)
            self.selectFrameLine.setText(str(cm.currentTime(q=True)))
            getFrameText = self.selectFrameLine.text()
            self.infoLabel.setText("Just "+getFrameText+" (single frame) you will export.")
            getSelectedFrame = cm.currentTime(self.selectFrameLine.text(), edit=True)     
            self.selectFrameLine.returnPressed.connect(self.printID)
     
        else:
            self.allFrames.setEnabled(True)
            self.frameRange.setEnabled(True)
            self.startFrame.setEnabled(True)
            self.endFrame.setEnabled(True)
            self.selectFrameLine.setText("")
            self.infoLabel.setText("")
   
    def allFramesOperation(self, state):
        if state == Qt.Checked:
            self.exportButton.clicked.connect(self.exportRedshiftProxy_allFrame)
            self.selectFrame.setEnabled(False)       
            self.frameRange.setEnabled(False)
            self.startFrame.setEnabled(False)
            self.endFrame.setEnabled(False)
            self.selectFrameLine.setEnabled(False)
            startTime = cm.getAttr ('defaultRenderGlobals.startFrame')
            endTime = cm.getAttr ('defaultRenderGlobals.endFrame')
            self.infoLabel.setText("You will get export between " + str(startTime) + " and " + str(endTime) + " frames.")                  
        else:
            self.selectFrame.setEnabled(True)
            self.frameRange.setEnabled(True)
            self.startFrame.setEnabled(True)
            self.endFrame.setEnabled(True)
            self.selectFrameLine.setEnabled(True)  
            self.infoLabel.setText("")
            
    def printID(self):
        self.infoLabel.clear()
        get_frame_new_num = getSelectedFrame = cm.currentTime(self.selectFrameLine.text(), edit=True) 
        asd = self.infoLabel.setText("Just "+str(get_frame_new_num)+" (single frame) you will export.")
        print asd
    
    def frameRangeOperation(self, state):
        if state == Qt.Checked:
            self.selectFrame.setEnabled(False)
            self.selectFrameLine.setEnabled(False)
            self.allFrames.setEnabled(False)
            self.infoLabel.setText("You put specific frame ranges as you can see.")
            self.exportButton.clicked.connect(self.exportRedshiftProxy_selectedFrames)

        else:
            self.selectFrame.setEnabled(True)       
            self.selectFrameLine.setEnabled(True)   
            self.allFrames.setEnabled(True)
            self.infoLabel.setText("")
            
##############################################################################################
          
    def filterList(self):
        self.listItem.clear()
        search_pattern = self.searchBar.text().lower()
        for nodes in self.listNodes:
            if search_pattern in nodes.lower():
                item = QListWidgetItem(nodes)
                self.listItem.addItem(item)

##### ALL FRAME
    def exportRedshiftProxy_allFrame(self):
        filename = cm.fileDialog2(fileFilter='*.rs',
                                    fileMode = 3,
                                    caption='Redshift Proxy Export')
        setStartFrame = cm.getAttr ('defaultRenderGlobals.startFrame')
        setEndFrame = cm.getAttr ('defaultRenderGlobals.endFrame')
        sceneName = os.path.basename(cm.file(q=1, sn=1)).replace(".ma","")
        for selectedNodes in filename:
            for i in cm.ls(sl=1):
                for a in range(1):      
                    if a == a or a<i:
                        cm.select(i)
                        for frameRange in range(int(setStartFrame), int(setEndFrame)+1):
                            currentTime = cm.currentTime(frameRange, edit=True)
                            file = cm.file(selectedNodes +"/"+ str(sceneName) + "_" + i + "_" + "####" + ".rs", type = "Redshift Proxy", force = True ,es = True ) 
        self.close()
        
##### SELECTED ONE FRAME OK
    def exportRedshiftProxy_currentFrame(self):
        filename = cm.fileDialog2(fileFilter='*.rs',
                                    fileMode = 3,
                                    caption="Redshift Proxy Export")
        sceneName = os.path.basename(cm.file(q=1, sn=1)).replace(".ma","")
        for filePath in filename:
            print filePath
        for i in cm.ls(sl=1):
            for a in range(1):      
                if a == a or a<i:
                    file = cm.file(filePath +"/" + i + ".rs", type = "Redshift Proxy", force = True ,es = True)
        self.close()    
                    
##### START END SELECTION
    def exportRedshiftProxy_selectedFrames(self):
        self.mainLayout.addWidget(self.progressBar)
        self.mainLayout.addWidget(self.mainProgressBar)
        filename = cm.fileDialog2(fileFilter="*.rs",
                                    fileMode = 3,
                                    caption="Redshift Proxy Export")
        selectedStartFrame = self.startFrame.text()
        selectedEndFrame = self.endFrame.text()
        sceneName = os.path.basename(cm.file(q=1, sn=1)).replace(".ma","")
        for selectedNodes in filename:
            for i in cm.ls(sl=1):
                for a in range(1):      
                    if a == a or a<i:
                        cm.select(i)
                        x = i.split("_").pop()
                        os.mkdir(selectedNodes+"/"+ x.replace(":","_"))            
                        for frameRange in range(int(selectedStartFrame), int(selectedEndFrame)+1):
                            currentTime = cm.currentTime(frameRange, edit=True)
                            o = i.split("_").pop()
                            file = cm.file(selectedNodes+"/"+x.replace(":","_") + "/" + "rsProxy" + "_" + o.replace(":","_") + "_" + "####" + ".rs", force = True, type = "Redshift Proxy", es = True )  
                            self.progressBar.setMinimum(int(self.startFrame.text()))          
                            self.progressBar.setFormat(i+": %p%")
                            self.progressBar.setMaximum(int(self.endFrame.text()))
                            self.progressBar.setValue(frameRange)


win = Form()
win.setWindowFlags(Qt.WindowStaysOnTopHint)
win.setWindowTitle("1000Volt: Redshift Proxy Export")
win.show()

