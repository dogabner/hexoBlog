---
layout: blog
title: Quick Export .fbx for Cinema4D
date: 2017-04-20 12:18:36
tags:
---
# 使用python脚本快速将模型导出为.fbx格式。

虽然是很简单的几句，没什么复杂的东西，实现的功能也仅仅是“保存文件”而已，但绑定到快捷键上之后，会让人产生一种Cinema4d与Unity无缝协作的错觉。虽然Unity兼容c4d的文件格式，但团队合作用c4d的格式果然还是不太方便。

{% codeblock python lang:python quickExport.py %}
import os
import c4d
from c4d import documents
def main():
    # 获取文件路径与文件名    
    doc = documents.GetActiveDocument()
    path = doc.GetDocumentPath()
    name = doc.GetDocumentName()
    # 将文件后缀改为.fbx
    if name[-3:] != "fbx":
        try:
            index = -1
            while name[index] != '.':
                index -= 1 
            name = name[:(index+1)]+"fbx"
            print "convert filetype to .fbx"
        except IndexError:
            print "maybe you need to save your file first"
            return
    
    path = os.path.join(path,name)
    # 导出模型到原文件路径
    if documents.SaveDocument(doc,path,c4d.SAVEDOCUMENTFLAGS_DONTADDTORECENTLIST,1026370):
        print "export document to: " + path
    else:
        print "export failed"
    c4d.EventAdd()    
if __name__=='__main__':
    main()
    
{% endcodeblock %}