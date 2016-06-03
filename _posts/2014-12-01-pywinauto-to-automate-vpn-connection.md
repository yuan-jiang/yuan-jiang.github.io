---
layout: post
title: Pywinauto to automate vpn connection
date: 2014-12-01 10:10:00 +0800
author: Yuan Jiang
tags: windows python automation
---

Automate vpn connection (Cisco IPSec VPN Client + SoftToken) using pywinauto package on windows platform.

{% highlight python %}
#!C:/Python27/pythonw.exe

__author__ = 'yuanjiang'

from pywinauto import application
import time

def get_token():
    token_app = application.Application()
    token_app.start_(r"C:\Program Files (x86)\Secure Computing\SofToken-II\SofToken-II.exe")
    token_app.SofToken.OK.Click()
    token_app.SofToken.Edit.TypeKeys("your-softtoken-pin")
    token_app.SofToken.Get_Password.Click()
    token = str(token_app.SofToken.Edit2.Texts[1])
    token_app.SofToken.Close.Click()
    return token

def start_vpn_connection(token):
    vpn_app = application.Application()
    try:
        vpn_app.connect_(path=r"C:\Program Files (x86)\Cisco Systems\VPN Client\vpngui.exe")
    except:
        vpn_app.start_(r"C:\Program Files (x86)\Cisco Systems\VPN Client\vpngui.exe")
    finally:
        vpn_app.connect_(path=r"C:\Program Files (x86)\Cisco Systems\VPN Client\vpngui.exe")

    # time.sleep(10)
    while True:
        try:
            if vpn_app.window_(title_re=".*VPN Client.*"):
                vpn_app.window_(title_re=".*VPN Client.*").qt_viewport.TypeKeys("{ENTER}") break
        except:
            pass

    # Handle authentication and enter the token
    while True:
        try:
            if vpn_app.window_(title_re=".*User Authentication.*"):
                vpn_app.window_(title_re=".*User Authentication.*").password.TypeKeys(token) vpn_app.window_(title_re=".*User Authentication.*").TypeKeys("{ENTER}")
                break
        except:
            pass

    # Dismiss the warning banner after successful connection
    while True:
        try:
            if vpn_app.window_(title_re=".*Banner"):
                vpn_app.window_(title_re=".*Banner").TypeKeys("{ENTER}")
                break
        except:
            pass

if __name__=="__main__":
    token = get_token()
    start_vpn_connection(token)
{% endhighlight %}
