#!/usr/bin/env python
# -*- coding: utf-8 -*-
#####################################################
import sys                                          #
import requests                                     # web sayfasına istek göndermek için
import sqlite3                                      # vt için 
import os                                           # 
import json                                         # json dosyaları için  
import base64                                       # dönüştürme işlemi için
import wordpress                                    # wordpress için
import webbrowser                                   # İnternet sayfası gösterimi için
import qdarkgraystyle                               # Renk/Tema değişimi için
#####################################################
from woocommerce import API                         # WooCommerce api si için kütüphane    
from PyQt5.Qt import *                              # Masaüstü arayüz uygulaması için 
from PyQt5.QtWebEngineWidgets import *              # 
from PyQt5.QtTest import *                          #
from PyQt5.QtWidgets import QApplication, QStyle    #
from io import BytesIO                              # İo kütüphanesindeki düzeltme için
from io import StringIO                             # 
from PyQt5 import QtGui                             # 
from PyQt5 import QtCore                            # 
from bs4 import BeautifulSoup as bs                 # verileri çekmek için 
#####################################################
baglanti = sqlite3.connect("siparisler.db")         # vt ye bağlanıldı
kalem = baglanti.cursor()                           #
#####################################################
minifont = QFont("Century Gothic", 14)              # yazı biçimleri
yaziFont = QFont("Century Gothic", 18)              #
baslikFont = QFont("Century Gothic", 25)            #
#####################################################
global sonyetkibuken                                #
############################################################################################################# Giriş ekranı - tasarım #

class Giris(QWidget):
    def __init__(self):
        super().__init__()

        self.setWindowTitle("DOĞAN DÜRÜM")
        self.bilgi = QLabel()
        self.bilgi.setFont(yaziFont)

        res = QLabel()
        res.setPixmap(QPixmap(os.getcwd() + "/logo.png"))
        yazi = QLabel("\tDOĞAN DÜRÜM")

        kullaniciadi = QLabel("KULLANICI ADI:")
        self.kulAdVeri = QLineEdit("yonetici")
        girisbilgi = QLabel("ŞİFRE :")
        self.sifre = QLineEdit("123")
        self.sifre.setEchoMode(QLineEdit.Password)
        girisbtn = QPushButton("GİRİŞ YAP")
        self.yenikul = QPushButton("YENİ KULLANICI EKLE")

        yazi.setFont(minifont)
        kullaniciadi.setFont(yaziFont)
        girisbilgi.setFont(yaziFont)
        girisbtn.setFont(yaziFont)
        self.yenikul.setFont(yaziFont)

        yerlestir = QVBoxLayout()
        yatay = QHBoxLayout()

        yerlestir.addStretch()
        yerlestir.addWidget(self.bilgi)
        yerlestir.addWidget(res)
        yerlestir.addWidget(yazi)
        yerlestir.addWidget(kullaniciadi)
        yerlestir.addWidget(self.kulAdVeri)
        yerlestir.addWidget(girisbilgi)
        yerlestir.addWidget(self.sifre)
        yerlestir.addStretch()
        yerlestir.addWidget(girisbtn)
        yerlestir.addWidget(self.yenikul)
        yerlestir.addStretch()

        yatay.addStretch()
        yatay.addLayout(yerlestir)
        yatay.addStretch()
        girisbtn.clicked.connect(self.giris_kontrol)
        self.yenikul.clicked.connect(self.yenikulekle)

        p = self.palette()
        p.setColor(self.backgroundRole(), Qt.white)
        self.setPalette(p)

        self.setLayout(yatay)
        self.show()
        self.setFixedSize(550, 550)
        
######################################################################################################################################

############################################################################################################ giriş anında yetkiyi al #
        
    def yetkiGonder(self):  # stop999
        kulladi = self.kulAdVeri.text()
        sifre = self.sifre.text()
        kontrolcuk = kalem.execute(
            """SELECT yetki FROM kullanicilar WHERE kulladi = '%s' AND sifre = '%s'""" % (kulladi, sifre))
        globals()['sonyetkibuken'] = kontrolcuk.fetchone()[0]
        print(sonyetkibuken)
        self.girisBasarili()
        
######################################################################################################################################

########################################################################################################### giriş bilgileri doğru mu #
        
    def giris_kontrol(self):                                                                                                
        kalem.execute("SELECT kulladi, sifre FROM kullanicilar")
        baglanti.commit()

        for res in kalem.fetchall():
            login = res[0]
            sifrevt = res[1]

            if self.kulAdVeri.text() == login:
                if self.sifre.text() == sifrevt:
                    self.yetkiGonder()
                else:
                    self.bilgi.setText("Kullanıcı Adı/şifre yanlış")
            else:
                self.bilgi.setText("Kullanıcı Adı/şifre yanlış")
                
######################################################################################################################################

################################################################################################# yeni kullanıcı sayfasına yönlendir #
                
    def yenikulekle(self):
        self.yenisayfa = YeniKulEkleSyf()
        self.yenisayfa.show()
        
######################################################################################################################################

######################################################################################################### ana ekrana yönlendirme yap #
        
    def girisBasarili(self):
        self.giris = Pencere()
        self.giris.show()
        Giris.close(self)
        
######################################################################################################################################

##################################################################################################### yeni kullanıcı ekranı - tasarım #
        
class YeniKulEkleSyf(QWidget):
    def __init__(self):
        super().__init__()

        self.setWindowTitle("KULLANICI EKLE")

        tepsi = QGridLayout()
        yatay = QVBoxLayout()

        self.kulekran = QLabel()
        isimtxt = QLabel("Kullanıcı Adi : ")
        sifretxt = QLabel("Şifre : ")
        self.isim = QLineEdit()
        self.sifre = QLineEdit()
        emailtxt = QLabel("Email : ")
        self.email = QLineEdit()
        teltxt = QLabel("Telefon No : ")
        self.tel = QLineEdit()
        self.kayitbtn = QPushButton("KULLANICI EKLE")

        self.kulekran.setFont(yaziFont)
        emailtxt.setFont(yaziFont)
        teltxt.setFont(yaziFont)
        isimtxt.setFont(yaziFont)
        sifretxt.setFont(yaziFont)
        self.kayitbtn.setFont(yaziFont)

        tepsi.addWidget(isimtxt, 0, 0)
        tepsi.addWidget(self.isim, 0, 1)
        tepsi.addWidget(sifretxt, 1, 0)
        tepsi.addWidget(self.sifre, 1, 1)
        tepsi.addWidget(emailtxt, 2, 0)
        tepsi.addWidget(self.email, 2, 1)
        tepsi.addWidget(teltxt, 3, 0)
        tepsi.addWidget(self.tel, 3, 1)
        tepsi.addWidget(self.kayitbtn, 4, 1)

        p = self.palette()
        p.setColor(self.backgroundRole(), Qt.white)
        self.setPalette(p)

        self.kayitbtn.clicked.connect(self.veriKontrol)

        yatay.addWidget(self.kulekran)
        yatay.addLayout(tepsi)
        self.setLayout(yatay)
        self.setFixedSize(500, 300)
        
######################################################################################################################################

###################################################################################################################### kayıt kontrol #
        
    def veriKontrol(self):
        kullad = self.isim.text()
        sifre = self.sifre.text()
        telno = self.tel.text()
        email = self.email.text()
        if (not kullad) or (not sifre):                                                                             # ekleme yapılabilir
            self.kulekran.setText("EKSİK VERİ GİRDİNİZ...")
        else:
            kalem.execute("INSERT INTO kullanicilar(kulladi,sifre,telno,email) VALUES (?, ?, ?, ?)",
                          (kullad, sifre, telno, email))
            baglanti.commit()
            self.kulekran.setText("KULLANICI KAYDEDİLDİ")
            
######################################################################################################################################

################################################################################################ manuel veri ekleme alanı - tasarımı #
            
class VeriEkleme(QWidget):
    def __init__(self):
        super().__init__()
        yazilar = QVBoxLayout()
        inputlar = QVBoxLayout()
        legoyap = QHBoxLayout()
        birlestir = QVBoxLayout()

        self.setWindowTitle("SİPARİŞ EKLEME")

        self.idtxt = QLabel("Sipariş ID: ")
        self.isimtxt = QLabel("İsim Soyisim: ")
        self.telnotxt = QLabel("Telefon no: ")
        self.adrestxt = QLabel("Adres : ")
        self.uruntxt = QLabel("Ürün Adi : ")
        self.maliyetxt = QLabel("Ürün başı fiyat : ")
        self.adetxt = QLabel("Adet : ")
        self.ekle = QPushButton("SİPARİS BİLGİSİ\n"
                                "EKLE")

        self.ekle.clicked.connect(self.kontrol)

        self.idtxt.setFont(yaziFont)
        self.isimtxt.setFont(yaziFont)
        self.telnotxt.setFont(yaziFont)
        self.adrestxt.setFont(yaziFont)
        self.uruntxt.setFont(yaziFont)
        self.maliyetxt.setFont(yaziFont)
        self.adetxt.setFont(yaziFont)
        self.ekle.setFont(yaziFont)

        self.id = QLineEdit("100")
        self.isim = QLineEdit("dogan adanalı")
        self.telno = QLineEdit("05309113030")
        self.adres = QLineEdit("Hurriyet mah. armada evleri")
        self.urun = QLineEdit("Urun Adı")
        self.maliyet = QLineEdit("6.5")
        self.adet = QLineEdit("2")

        yazilar.addWidget(self.idtxt)
        yazilar.addWidget(self.isimtxt)
        yazilar.addWidget(self.telnotxt)
        yazilar.addWidget(self.adrestxt)
        yazilar.addWidget(self.uruntxt)
        yazilar.addWidget(self.maliyetxt)
        yazilar.addWidget(self.adetxt)

        inputlar.addWidget(self.id)
        inputlar.addWidget(self.isim)
        inputlar.addWidget(self.telno)
        inputlar.addWidget(self.adres)
        inputlar.addWidget(self.urun)
        inputlar.addWidget(self.maliyet)
        inputlar.addWidget(self.adet)

        legoyap.addLayout(yazilar)
        legoyap.addLayout(inputlar)

        birlestir.addLayout(legoyap)

        birlestir.addWidget(self.ekle)

        p = self.palette()
        p.setColor(self.backgroundRole(), Qt.white)
        self.setPalette(p)

        self.setLayout(birlestir)
        
######################################################################################################################################

    def kontrol(self):
        if (
                self.id.text() == "" or self.isim.text() == "" or self.telno.text() == "" or self.adres.text() == "" or self.urun.text() == "" or self.maliyet.text() == "" or self.adet.text() == ""):
            QMessageBox.warning(self, "UYARI", "EKSİK VERİ GİRDİZ", QMessageBox.Ok)
        else:
            gecici = kalem.execute(
                "SELECT * FROM gecici_bilgi")                                   # gecici_bilgi deki verileri çek

            if len(gecici.fetchall()) > 0:                                      # gecici_bilgi tablosunda bilgi var ise uyarı veriyor.
                self.uyari()
            else:
                self.toplam = float(self.maliyet.text()) * float(self.adet.text())
                self.ekleMesaj2()

############################################################################################################### veri var mı - yok mu #
                
    def uyari(self):
        self.msg = QMessageBox()
        self.msg.setWindowTitle("UYARI")
        self.msg.setText("ZATEN VERİLER GİRİLMİŞ...")
        self.msg.setIcon(QMessageBox.Warning)
        self.msg.setStandardButtons(QMessageBox.Ok)
        self.msg.setDefaultButton(QMessageBox.Ok)
        self.msg.show()
        
######################################################################################################################################

######################################################################### girilen sipariş bilgilerini gecici_bilgi vt tablosuna ekle #
        
    def ekleMesaj2(self):
        try:
            idbilgi = int(self.id.text())
            isimbilgi = self.isim.text()
            telnobilgi = self.telno.text()
            adresbilgi = self.adres.text()
            urunbilgi = self.urun.text()
            maliyetbilgi = float(self.maliyet.text())
            adetbilgi = int(self.adet.text())
            toplambilgi = self.toplam

            QTest.qWait(750)
            kalem.execute(
                "INSERT OR IGNORE INTO gecici_bilgi(id,isim,telno,adres,urun_adi,maliyet,adet,toplam) VALUES (?, ?, ?, ?, "
                "?, ?, ?, ?)",
                (idbilgi, isimbilgi, telnobilgi, adresbilgi, urunbilgi, maliyetbilgi, adetbilgi, toplambilgi))

            QTest.qWait(1000)
            baglanti.commit()

            self.siparisEklendi()
        except:                                                                                                     # bir sorun oluştu
            QMessageBox.warning(self, "UYARI", "BİR HATA OLUŞTU\n"
                                               "Hata kodu = c_VE.d_E2-300", QMessageBox.Ok)
            
######################################################################################################################################
            
############################################################################################# manuel sipariş eklendi - aşama 1 bitti #
            
    def siparisEklendi(self):
        self.msg = QMessageBox()
        self.msg.setWindowTitle("SİPARİŞ EKLENDİ ")
        self.msg.setText("SİPARİŞ BİLGİSİ ALINDI DİĞER DETAYLARI GİRİNİZ VE KAYDET E BASINIZ")
        self.msg.setIcon(QMessageBox.Information)
        self.msg.setStandardButtons(QMessageBox.Ok)
        self.msg.setDefaultButton(QMessageBox.Ok)
        self.msg.show()
        
######################################################################################################################################

########################################################################################################################## Ana ekran #
        
class Pencere(QWidget):
    def __init__(self):
        super().__init__()

        self.toplamList = []
        self.miktarList = []
        self.urunAdiList = []
        self.fiyatList = []
        
######################################################################################################################################

####################################################################################################################### yönetici mi? #
        
        self.yetki = False

        if sonyetkibuken == 1:
            self.yetki = True
        if self.yetki:
            yoneticiad = "- YÖNETİCİ"
        else:
            yoneticiad = "- KULLANICI"
            
######################################################################################################################################

#################################################################################################################### uygulama düzeni #
            
        self.yenileicon = "yenile_64.png"
        self.kaydeticon = "save_64.png"

        gridtepsi = QGridLayout()

        self.orta = QVBoxLayout()
        self.soltaraf = QVBoxLayout()
        self.test = QHBoxLayout()

        sagUstKose = QHBoxLayout()

        self.setWindowTitle("DOĞAN DÜRÜM " + yoneticiad)
        self.baslik = QLabel("KAYIT ALANI")
        self.baslik.setFont(baslikFont)

        self.baslik2 = QLabel("WEB SİTE SİPARİŞ ALANI")
        self.baslik2.setFont(baslikFont)

        self.listebaslik = QLabel("SİPARİŞ LİSTESİ")
        self.listebaslik.setFont(baslikFont)

        self.web = QWebEngineView()
        self.web.load(
            QUrl("https://www.google.com"))                                                             # sitenin wordpress giriş urlsi 

        self.grupRadyo = QGroupBox()
        self.radyoLay = QVBoxLayout()
        self.radyo1 = QRadioButton("YENİ SİPARİŞ EKLE")
        self.radyo2 = QRadioButton("GÖNDERİM BAŞARILI")
        self.radyo3 = QRadioButton("İPTAL/İADE")
        self.radyoLay.addWidget(self.radyo1)
        self.radyoLay.addWidget(self.radyo2)
        self.radyoLay.addWidget(self.radyo3)
        self.grupRadyo.setLayout(self.radyoLay)

        self.radyo1.toggled.connect(self.toggleRadio)
        self.radyo2.toggled.connect(self.toggleRadio)
        self.radyo3.toggled.connect(self.toggleRadio)

        self.otoal = QPushButton("SON SİPARİŞİN \n"
                                 ">>> OTO BİLGİSİNİ AL>>>")
        self.label1 = QLabel("BİGİLENDİRME : ")
        self.ekran = QLabel("")
        self.liste = QListWidget()
        self.detay = QLabel("EK NOT :")
        self.detaytext = QLineEdit("İsteğe bağlı Ek Bilgi")
        self.detaytext.setPlaceholderText("EK NOT EKLEYİN...")
        self.kaydet = QPushButton("KAYDET")
        self.yenile = QPushButton("YENİLE")
        self.yeniekle = QPushButton("MANUEL BİLGİ GİR")

        self.kaydet.setIcon(QtGui.QIcon(self.kaydeticon))
        self.kaydet.setIconSize(QtCore.QSize(48, 48))
        self.yenile.setIcon(QtGui.QIcon(self.yenileicon))
        self.yenile.setIconSize(QtCore.QSize(64, 64))

        self.label1.setFont(yaziFont)
        self.ekran.setFont(minifont)
        self.radyo1.setFont(yaziFont)
        self.radyo2.setFont(yaziFont)
        self.radyo3.setFont(yaziFont)
        self.otoal.setFont(yaziFont)
        self.detay.setFont(yaziFont)
        self.kaydet.setFont(yaziFont)
        self.yenile.setFont(yaziFont)
        self.yeniekle.setFont(yaziFont)

        self.yeniekle.clicked.connect(self.kontrol)
        self.kaydet.clicked.connect(self.kayitList)
        self.yenile.clicked.connect(self.f5)
        self.liste.itemClicked.connect(self.siparisdetay)
        self.otoal.clicked.connect(self.otoVeri)

        self.orta.addStretch()
        self.orta.addWidget(self.label1)
        self.orta.addWidget(self.ekran)
        self.orta.addWidget(self.otoal)
        self.orta.addStretch()
        self.orta.addWidget(self.detay)
        self.orta.addWidget(self.detaytext)
        self.orta.addStretch()
        self.orta.addWidget(self.grupRadyo)
        self.orta.addStretch()
        self.orta.addWidget(self.yeniekle)
        self.orta.addWidget(self.kaydet)
        self.orta.addStretch()

        self.grupbox = QGroupBox()

        self.soltaraf.addWidget(self.baslik2)
        self.soltaraf.addWidget(self.web)

        deneme = QVBoxLayout()
        
######################################################################################################################################

##################################################################################################### menü kısmı başlangıc - tasarım #
        
        menubar = QMenuBar()
        self.girisAction = QAction(self.style().standardIcon(QStyle.SP_FileDialogBack), 'Hesaptan Çıkış Yap')
        self.girisAction.setShortcut('Ctrl+Q')
        self.girisAction.triggered.connect(self.basUyari)

        self.yeniSiparisAction = QAction(self.style().standardIcon(QStyle.SP_FileDialogNewFolder), 'Yeni Sipariş Ekle')
        self.yeniSiparisAction.setShortcut('Ctrl+Y')
        self.yeniSiparisAction.triggered.connect(self.veriAc)

        self.siparisListeAction = QAction(self.style().standardIcon(QStyle.SP_FileDialogContentsView),
                                          'Sipariş Listesi')
        self.siparisListeAction.setShortcut('Ctrl+L')
        self.siparisListeAction.triggered.connect(self.listeAc)

        self.arkaAction = QAction(self.style().standardIcon(QStyle.SP_FileDialogToParent),
                                  "Renk Değiştir")
        if self.yetki:
            self.arkaAction.triggered.connect(self.renkDegis)
            self.renk = False
        else:
            self.arkaAction.triggered.connect(self.yetkiUyari)

        self.deeme = QAction(self.style().standardIcon(QStyle.SP_FileDialogInfoView),
                             "Yardım")
        self.deeme.triggered.connect(self.linkAc)

        self.webAction = QAction(self.style().standardIcon(QStyle.SP_DialogOpenButton),
                                 "Web Sitesi")
        self.webAction.triggered.connect(self.siteAc)
        actionGiris = menubar.addMenu("Giriş")
        actionGiris.addAction(self.girisAction)
        actionSiparis = menubar.addMenu("Siparişler")
        actionSiparis.addAction(self.yeniSiparisAction)
        actionSiparis.addSeparator()
        actionSiparis.addAction(self.siparisListeAction)
        actionArkaplan = menubar.addMenu("Ayarlar")
        actionArkaplan.addAction(self.arkaAction)
        actionIletisim = menubar.addMenu("İletişim")
        actionIletisim.addAction(self.deeme)
        actionSiparis.addSeparator()
        actionIletisim.addAction(self.webAction)
        
######################################################################################################################################

########################################################################################################### ekran düzenini yerleştir #

        sagUstKose.addWidget(self.listebaslik)
        sagUstKose.addStretch()
        sagUstKose.addWidget(self.yenile)

        gridtepsi.addWidget(menubar, 0, 0)
        gridtepsi.addWidget(self.baslik2, 1, 0)
        gridtepsi.addWidget(self.web, 2, 0)
        gridtepsi.addWidget(self.baslik, 1, 1)
        gridtepsi.addLayout(self.orta, 2, 1)
        gridtepsi.addLayout(sagUstKose, 1, 2)
        gridtepsi.addWidget(self.liste, 2, 2)

        deneme.addLayout(gridtepsi)
        self.setStyleSheet(qdarkgraystyle.load_stylesheet())
        
        self.f5()

        self.setLayout(deneme)
        self.resize(1400, 800)
        self.show()
        
######################################################################################################################################

######################################################################################################################### yetkin yok #
        
    def yetkiUyari(self):
        self.msgUy = QMessageBox()
        self.msgUy.setWindowTitle("UYARI")
        self.msgUy.setText("DİKKAT : YÖNETİCİ DEĞİLSİNİZ")
        self.msgUy.setDetailedText("YETKİNİZ YOK")
        self.msgUy.setIcon(QMessageBox.Critical)
        self.msgUy.setStandardButtons(QMessageBox.Ok)
        self.msgUy.show()
        
######################################################################################################################################

###################################################################################################################### renk değiştir #
        
    def renkDegis(self):
        if self.renk:
            self.setStyleSheet(qdarkgraystyle.load_stylesheet())
            self.renk = False
        else:
            self.setStyleSheet("""
                            QWidget {
                                border: 2px solid black;
                                border-radius: 10px;
                                background-color: rgb(255, 255, 255);
                                }
                            """)
            self.renk = True
            
######################################################################################################################################

##################################################################################################################### yönlendirmeler #
            
    def siteAc(self):
        webbrowser.open('https://www.google.com', new=2) # yönlendirilecek web sitesi new = 2 yeni sekmede açar

    def linkAc(self):
        webbrowser.open('https://www.instagram.com/dogan.adanali', new=2)
        self.msgUy = QMessageBox()
        self.msgUy.setWindowTitle("SOSYAL MEDYA ")
        self.msgUy.setText("Sosyal medyadan bizimle iletişime geçebilirsiniz.")
        self.msgUy.setIcon(QMessageBox.Warning)
        self.msgUy.setStandardButtons(QMessageBox.Ok | QMessageBox.Cancel)
        self.msgUy.show()
        
######################################################################################################################################

################################################################################################ girişe dönmek istediğine emin misin #
        
    def basUyari(self):
        msgUy = QMessageBox()
        msgUy.setWindowTitle("DİKKAT")
        msgUy.setText("GİRİŞ EKRANINA GİTMEK İSTİYOR MUSUNUZ ? ")
        msgUy.setIcon(QMessageBox.Warning)
        msgUy.setStandardButtons(QMessageBox.Ok | QMessageBox.Cancel)

        deger = int(msgUy.exec_())
        if deger == 1024:
            self.basaDon()
        msgUy.show()
        
######################################################################################################################################
    
################################################################################################################# Giriş ekranına dön #
        
    def basaDon(self):
        self.bas = Giris()
        self.close()
        self.bas.show()
        
######################################################################################################################################
        
########################################################################################################### wooCoommerce ayarını yap #
        
    def otoVeri(self):                                                                                      # acele etme
        self.ekran.setText("LÜTFEN BEKLEYİNİZ KAYIT YAPILIYOR")                                                         
        QTest.qWait(500)

        wcapi = API(
            url="https://www.google.com",
            consumer_key="",                                                                                # woocommerce api user key
            consumer_secret="",                                                                             # woocommerce api secret key 
            version="wc/v3",
            timeout=100
        )

        siparisler = wcapi.get("orders").json()

        self.urnjson = siparisler[0]["line_items"]                                                          # son siparişin ürünleri                    
        urunlist = []
        
######################################################################################################################################

############################################################################################ json dosyasından verileri listelere çek #
        
        for i in range(len(self.urnjson)):                              

            self.urunAdiList.append(self.urnjson[i]["name"])
            urunlist.append("\n>> ÜRÜN : >> " + self.urnjson[i]["name"])

            self.fiyatList.append(str(self.urnjson[i]["price"]))
            urunlist.append("MALİYET : " + str(self.urnjson[i]["price"]) + " TL ")

            self.miktarList.append(str(self.urnjson[i]["quantity"]))
            urunlist.append(str(self.urnjson[i]["quantity"]) + " ADET")

            self.toplamList.append(str(self.urnjson[i]["total"]))
            urunlist.append("TOPLAM FİYAT : " + self.urnjson[i]["total"] + " TL ")
            
######################################################################################################################################
            
############################################################################ Web Siteden json dosyasından son sipariş bilgilerini al #
            
        idjson = siparisler[0]["id"]
        self.faturajson = siparisler[0]["billing"]
        isimjson = self.faturajson["first_name"] + " " + self.faturajson["last_name"]
        telnojson = self.faturajson["phone"]
        adresjson = self.faturajson["address_1"]
        urunStr = "\n".join(self.urunAdiList)
        fiyatStr = "\n".join(self.fiyatList)
        miktarStr = "\n".join(self.miktarList)
        toplamStr = "\n".join(self.toplamList)
        dipnotOto = "WEB SİTEDEN OTOMATİK ALINMIŞ BİLGİDİR."
        sonuc = siparisler[0]["status"]
        
######################################################################################################################################
        
################################################################################################# Verileri veritabanına ekle (yoksa) #
    
        kalem.execute(
            "INSERT OR IGNORE INTO siparis_alani(id,isim,telno,adres,urun_adi,maliyet,adet,toplam,dipnot,"
            "sonuc) VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?)",
            (idjson, isimjson, telnojson, adresjson, urunStr, fiyatStr, miktarStr, toplamStr, dipnotOto, sonuc))
        
######################################################################################################################################

##################################################################################################### bekleme ve listeleri temizleme #
        
        QTest.qWait(500)
        baglanti.commit()

        self.ekran.setText("KAYIT BAŞARILI")
        self.f5()                                                                                                           # yenile          
        self.urunAdiList.clear()
        self.fiyatList.clear()
        self.miktarList.clear()
        self.toplamList.clear()
        
######################################################################################################################################

########################################################################################## listede tıklanan item in detaylarını çek #
        
    def siparisdetay(self, item):
        self.isim = item.text()
        b = kalem.execute("SELECT * FROM siparis_alani WHERE isim = (?)", (self.isim,))
        baglanti.commit()
        self.a = b.fetchone()
        self.bilgiGoster()
        
######################################################################################################################################

############################################################################################## Tıklanan item in detaylarını gösterir #
        
    def bilgiGoster(self):

        self.msg = QMessageBox()
        self.msg.setWindowTitle("SİPARİŞ BİLGİ")
        self.msg.setText("Sipariş Detayı İçin 'SHOW DETAİLS' \n"
                         "Siparişi silmek için 'ABORT' \n"
                         "Kapatmak için 'CANCEL' a Tıklayınız.. ")
        self.msg.setIcon(QMessageBox.Question)
        self.msg.setStandardButtons(QMessageBox.Cancel | QMessageBox.Abort)
        self.msg.setDefaultButton(QMessageBox.Cancel)

        self.msg.setDetailedText("ID     \t{}\n"
                                 "İSİM   \t{}\n"
                                 "TELEFON\t{}\n"
                                 "ADRES  \t{}\n"
                                 "URUN   \t{}\n"
                                 "FİYATI \t{}\n"
                                 "ADET   \t{}\n"
                                 "TOPLAM \t{}\n"
                                 "EK NOT \t{}\n"
                                 "İSLEM  \t{}".format(self.a[0], self.a[1], self.a[2], self.a[3], self.a[4], self.a[5],
                                                      self.a[6], self.a[7], self.a[8], self.a[9]))
        
######################################################################################################################################

############################################################################################## abort tuşuna basılırsa vt den silinir #
        
        deger = int(self.msg.exec_())
        if deger == 262144:                                                 # abort tuşunun int değeri --> QMessageBox.Abort == 262144

            sil = kalem.execute("DELETE FROM siparis_alani WHERE isim = (?)", (self.isim,))

            self.msg.setText(self.isim + " ADLI KİŞİNİN SİPARİŞİ SİLİNMİŞTİR")
            listecek = kalem.execute("SELECT * FROM siparis_alani")
            baglanti.commit()
            self.liste.clear()
            for k in listecek.fetchall():
                self.liste.addItem(k[1])
                
######################################################################################################################################

        self.msg.resize(500, 300)                                                                               # boyutlandır ve göster
        self.msg.show()

#################################################################################################################### listeyi yeniler #
        
    def f5(self):
        listecek = kalem.execute("SELECT * FROM siparis_alani")
        self.liste.clear()
        for k in listecek.fetchall():
            self.liste.addItem(k[1])
            
######################################################################################################################################

########################################################################################### gecici_bilgi vt tablosunda veri var mı ? #
            
    def kontrol(self):
        gecici = kalem.execute("SELECT * FROM gecici_bilgi")  
        if len(gecici.fetchall()) > 0:
            self.uyari()
        else:
            self.veriAc()
            
######################################################################################################################################

################################################################################################### manuel veri ekleme ekranı açılır #
            
    def veriAc(self):                                                                                           # liste ekranını açılır
        self.veri = VeriEkleme()                                                                                # uyarı ekranı açılır
        self.veri.show()

    def listeAc(self):
        self.yeniEkran = SiparisListesi()
        self.yeniEkran.show()

    def uyari(self):
        self.msg = QMessageBox()
        self.msg.setWindowTitle("UYARI")
        self.msg.setText("ZATEN VERİLER GİRİLMİŞ...")
        self.msg.setIcon(QMessageBox.Warning)
        self.msg.setStandardButtons(QMessageBox.Ok)
        self.msg.setDefaultButton(QMessageBox.Ok)
        self.msg.show()
        
######################################################################################################################################
        
################################################################################################# radio btn tıklanır ise #############
        
    def toggleRadio(self):                                                                      # radio btn deki yazıların atamasını yap
        rdButon = self.sender()
        if rdButon.isChecked():
            if rdButon.text() == "YENİ SİPARİŞ EKLE":
                self.radyodeger = "Yeni Sipariş"
            elif rdButon.text() == "GÖNDERİM BAŞARILI":
                self.radyodeger = "Tamamlandı"
            else:
                self.radyodeger = "İPTAL/İADE"
            print(self.radyodeger)
            
######################################################################################################################################

###################################################################################### Girilen verileri vt tablosuna kaydet ##########
            
    def kayitList(self):

        try:
            geciciKontrol = kalem.execute("SELECT * FROM gecici_bilgi")
            QTest.qWait(1000)
            baglanti.commit()
            self.ekran.setText("!!\t!!\t!!\t!!\t!!")
            
            bilgi = geciciKontrol.fetchall()
            self.idcek = bilgi[0][0]
            self.isimcek = bilgi[0][1]
            self.telnocek = bilgi[0][2]
            self.adrescek = bilgi[0][3]
            self.uruncek = bilgi[0][4]
            self.malicek = bilgi[0][5]
            self.adetcek = bilgi[0][6]
            self.topcek = bilgi[0][7]
            detaybilgi = self.detaytext.text()
            QTest.qWait(750)
            kalem.execute(
                "INSERT INTO siparis_alani(id,isim,telno,adres,urun_adi,maliyet,adet,toplam,dipnot,sonuc) VALUES (?, ?, "
                "?, ?, ?, ?, ?, ?, ?, ?)", (self.idcek, self.isimcek, self.telnocek, self.adrescek, self.uruncek,
                                            self.malicek, self.adetcek,
                                            self.topcek, detaybilgi, self.radyodeger))

            QTest.qWait(1000)
            kalem.execute("DELETE FROM gecici_bilgi")

            QTest.qWait(1000)
            listecek = kalem.execute("SELECT * FROM siparis_alani")
            baglanti.commit()
            self.liste.clear()
            for k in listecek.fetchall():
                self.liste.addItem(k[1])
            self.ekran.setText("SİPARİŞ EKLENDİ")
            
######################################################################################################################################

####################################################################################################### veriler boş ise uyarı ekranı #
            
        except:
            self.msg = QMessageBox()
            self.msg.setWindowTitle("UYARI ")
            self.msg.setText("ÖNCE MANUEL BİLGİ GİRİNİZ")
            self.msg.setIcon(QMessageBox.Warning)
            self.msg.setStandardButtons(QMessageBox.Cancel)
            self.msg.setDefaultButton(QMessageBox.Cancel)
            self.msg.resize(200, 100)
            self.msg.show()
            self.ekran.setText("!!\t!!\t!!\t!!\t!!")
            QTest.qWait(400)
            self.ekran.setText("ÖNCE MANUEL BİLGİ GİRİNİZ")
            
######################################################################################################################################

############################################################################################## Sipariş listesi yeni pencere- tasarım #
            
class SiparisListesi(QWidget):
    def __init__(self):
        super().__init__()

        self.setWindowTitle("DOĞAN DÜRÜM - SİPARİŞ LİSTESİ")

        yatay = QVBoxLayout()

        self.listebaslik = QLabel("SİPARİŞ LİSTESİ")
        self.listebaslik.setFont(baslikFont)
        self.liste = QListWidget()
        self.yenile = QPushButton("YENİLE")
        self.yenile.setFont(baslikFont)

        yatay.addWidget(self.listebaslik)
        yatay.addWidget(self.liste)
        yatay.addWidget(self.yenile)

        self.yenile.clicked.connect(self.f5le)
        self.liste.itemClicked.connect(self.siparisdetay)
        self.f5le()

        self.setLayout(yatay)
        self.resize(500, 500)
        self.show()
        
######################################################################################################################################

#################################################################################################################### yenileme ekranı #
        
    def f5le(self):

        listecek = kalem.execute("SELECT * FROM siparis_alani")
        self.liste.clear()

        for k in listecek.fetchall():
            self.liste.addItem(k[1])
            print(k[1])
            
######################################################################################################################################

######################################################################################################### tıklanan item in bilgileri #
            
    def siparisdetay(self, item):
        self.isim = item.text()
        b = kalem.execute("SELECT * FROM siparis_alani WHERE isim = (?)", (self.isim,))
        baglanti.commit()
        self.a = b.fetchone()
        self.bilgiGoster()
        
######################################################################################################################################
        
################################################################################################ Listedeki bilgileri ekrana gösterir #
        
    def bilgiGoster(self):

        self.msg = QMessageBox()
        self.msg.setWindowTitle("SİPARİŞ BİLGİ")
        self.msg.setText("Sipariş Detayı İçin 'SHOW DETAİLS' \n"
                         "Siparişi silmek için 'ABORT' \n"
                         "Kapatmak için 'CANCEL' a Tıklayınız.. ")
        self.msg.setIcon(QMessageBox.Question)
        self.msg.setStandardButtons(QMessageBox.Cancel | QMessageBox.Abort)
        self.msg.setDefaultButton(QMessageBox.Cancel)

        self.msg.setDetailedText("ID     \t{}\n"
                                 "İSİM   \t{}\n"
                                 "TELEFON\t{}\n"
                                 "ADRES  \t{}\n"
                                 "URUN   \t{}\n"
                                 "FİYATI \t{}\n"
                                 "ADET   \t{}\n"
                                 "TOPLAM \t{}\n"
                                 "EK NOT \t{}\n"
                                 "İSLEM  \t{}".format(self.a[0], self.a[1], self.a[2], self.a[3], self.a[4], self.a[5],
                                                      self.a[6], self.a[7], self.a[8], self.a[9]))
        deger = int(self.msg.exec_())

        if deger == 262144:                                                 # abort tuşunun int değeri --> QMessageBox.Abort == 262144

            sil = kalem.execute("DELETE FROM siparis_alani WHERE isim = (?)", (self.isim,))

            self.msg.setText(self.isim + " ADLI KİŞİNİN SİPARİŞİ SİLİNMİŞTİR")
            listecek = kalem.execute("SELECT * FROM siparis_alani")
            baglanti.commit()
            self.liste.clear()
            for k in listecek.fetchall():
                self.liste.addItem(k[1])

        self.msg.resize(500, 300)
        self.msg.show()
        
######################################################################################################################################

uygulama = QApplication(sys.argv)
pencere = Giris()
sys.exit(uygulama.exec_())

############################################# 4 Tab menü ye kullanıcı ekleme - silme - kullanıcı yetki ayarlaması yap --> yönetici ise
