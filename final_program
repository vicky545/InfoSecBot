import telebot
import time


#for generating
import string
from random import *


#for database connecting
import pymysql.cursors
connection = pymysql.connect(host='localhost', user='root', password='', db='',charset='utf8mb4',cursorclass=pymysql.cursors.DictCursor)
cursor = connection.cursor()


#logger
import logging
logger = logging.getLogger(__name__)


#decl


uid=[]
tid=""
flag=dict()
s="SELECT userid from auc"
cursor.execute(s)
r=cursor.fetchall()
i=0
for a in r:
		uid.append(r[i]['userid'])
		flag[r[i]['userid']]=0
		i=i+1
bot_token='TOKEN'
bot=telebot.TeleBot(token=bot_token)

@bot.message_handler(commands=['start','START'])
def send_welcome(message):
	global tid,flag,uid
	s="SELECT userid from auc"
	cursor.execute(s)
	r=cursor.fetchall()
	i=0
	print(uid,flag)
	bot.reply_to(message,"""\
    Welcome :)
    This is a bot to store passwords and generate very strong passwords
    if you want to generate a new password""")
	try:
		t5=message.text.split()
		password5=t5[1]
		tid = str(message.from_user.id)
		if tid in uid:
			try:
				sql="SELECT password from auc where userid='%s'"%(tid)
				cursor.execute(sql)
				row=cursor.fetchone()
				if row['password']==password5:
					flag[tid]=1
					bot.reply_to(message,"you can use the bot now")
				else:
					flag[tid]=0
					bot.reply_to(message,"Wrong Password please authenticate again")
			finally:
				pass
				print(flag)
		else:
			try:
				sql = "INSERT INTO auc (userid,password) values('%s','%s')"%(tid,password5)
				cursor.execute(sql)
				connection.commit()
				uid.append(tid)
				flag[tid]=1
			finally:
				pass
				bot.reply_to(message,'user created')
				print(flag)
	except:
		bot.reply_to(message,"authenticate using /start pass")






@bot.message_handler(commands=['help','HELP'])
def help(message):
	bot.reply_to(message,"""
     '/generate',
     '/store domain_name_tostore',
	 '/get domain_name'
	 '/update domain_name updatedpassword'
     for now.""")

@bot.message_handler(commands=['generate','GENERATE'])
def generate(message):


    characters = string.ascii_letters + string.punctuation  + string.digits
    password =  "".join(choice(characters) for x in range(randint(8, 16)))
    bot.reply_to(message,'a pretty strong password is')
    bot.reply_to(message,''+password)


@bot.message_handler(commands=['store','STORE'])
def store(message):
	global flag
	t=message.text.split()
	name=t[1]
	password=t[2]
	tid2= str(message.from_user.id)
	if flag[tid2]==1:
		try:
			sql = "INSERT INTO mini (userid,name,password) values('%s','%s' ,'%s')"%(tid2,name,password)
			cursor.execute(sql)
			connection.commit()
		finally:
			pass
			bot.reply_to(message,'password stored')
	else:
		bot.reply_to(message,"Please authenticate using '/start pass'")


@bot.message_handler(commands=['get','GET'])
def get(message):
	global flag
	t=message.text.split()
	a=t[1]
	tid1=str(message.from_user.id)
	try:
		if flag[tid1]==1:
			try:
				sql="SELECT * from mini where name='%s' and userid='%s'"%(a,tid1)
				cursor.execute(sql)
				row=cursor.fetchone()
				bot.reply_to(message,''+row['password'])
			except:
				bot.reply_to(message,'no such domain present or please authenticate using start')
			finally:
				pass
		else:
			bot.reply_to(message,"Please authenticate using '/start pass'")
	except:
		bot.reply_to(message,"Please authenticate using '/start pass'")


@bot.message_handler(commands=['gs','GS'])
def gs(message):
	global flag
	t1=message.text.split()
	name1=t1[1]
	characters1 = string.ascii_letters + string.punctuation  + string.digits
	password1 =  "".join(choice(characters1) for x in range(randint(8, 16)))
	tid3= str(message.from_user.id)
	if flag[tid3]==1:
		try:
			sql1 = "INSERT INTO mini (userid,name,password) values('%s','%s','%s')"%(tid3,name1,password1)
			cursor.execute(sql1)
			connection.commit()
		finally:
			pass
			bot.reply_to(message,'password generated and stored it is:')
			bot.reply_to(message,''+password1)
	else:
		bot.reply_to(message,"Please authenticate using '/start pass'")

@bot.message_handler(commands=['update','UPDATE'])
def update(message):
	global flag
	t2=message.text.split()
	name2=t2[1]
	password2=t2[2]
	tid4= str(message.from_user.id)
	if flag[tid4]==1:
		try:
			sql2="UPDATE mini SET password=%s where name=%s and userid=%s"
			cursor.execute(sql2,(password2,name2,tid4))
			connection.commit()
		finally:
			pass
			bot.reply_to(message,'password updated')
	else:
		bot.reply_to(message,"Please authenticate using '/start pass'")


while True:
	try:
		bot.polling()
	except Exception:
		time.sleep(15)
