from flask import Flask,render_template,Markup
import mraa 

app = Flask(__name__)

@app.route("/")
def index():
    #return("<h1> Welcome to Sampy's webpage </h1>")
    #bodyText=Markup("<bod>This is the body</b>")
    return render_template("new2.html") #,bodyText=bodyText)


@app.route("/first")
def first():
   pin=mraa.Aio(0)
   pin.setBit(12)
   rawReading=pin.read()
   volts=float(rawReading / 819.0)
   tempC=( volts * 100 ) - 50
   tempF=( tempC * 9.0 / 5.0 ) + 32.0
   return render_template("display_temp.html",name=tempF)

@app.route("/second")
def second():
    sw=mraa.Gpio(12)
    sw.dir(mraa.DIR_IN)
    if(sw.read()==1):
        status="pressed"
    else:
        status="not pressed"
    return render_template("read_button.html",name=status)


@app.route("/thirdo")
def third_0():
    led=mraa.Gpio(13)
    led.dir(mraa.DIR_OUT)
    led.write(0)
    return render_template("ctl_led.html")

@app.route("/thirdi")
def third_1():
    led=mraa.Gpio(13)
    led.dir(mraa.DIR_OUT)
    led.write(1)
    return render_template("ctl_led.html")

@app.route("/fourth")
def fourth():
    led_status=mraa.Gpio(2)
    led_status.dir(mraa.DIR_IN)
    led_2_status=mraa.Gpio(8)
    led_2_status.dir(mraa.DIR_IN)
    if(led_status.read()==1):
        status="ON"
    else:
        status="OFF"
    if(led_2_status.read()==1):
        status_2="ON"
    else:
        status_2="OFF"
    return render_template("led_status.html", status=status,status_2=status_2)


@app.route("/fiftho")
def fifth_0():
    led=mraa.Gpio(7)
    led.dir(mraa.DIR_OUT)
    led.write(0)
    return render_template("ctl_led_2.html")



@app.route("/fifthi")
def fifth_1():
    led=mraa.Gpio(7)
    led.dir(mraa.DIR_OUT)
    led.write(1)
    return render_template("ctl_led_2.html")


if __name__=="__main__":
    app.run(host="0.0.0.0")
