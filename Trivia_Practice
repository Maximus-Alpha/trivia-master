#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import requests, re, smtplib, time

from bs4 import BeautifulSoup

import pandas as pd

from itertools import zip_longest

from email.message import EmailMessage


#Part 1: Gather the information


#Get the trivia questions
url = 'https://www.opinionstage.com/blog/trivia-questions/'
html_text = requests.get(url).text
soup=BeautifulSoup(html_text,'html.parser')

#Print a response code --> 200 means we've accessed the page
print("Response Code: {}".format(requests.get(url)))

#Get the questions from this page
questions = [things.text for item in soup.find_all('p') for things in item.find_all('strong') if re.search("^\d+.\s*",things.text)]

#Get the answers
answers = [item.text for item in soup.find_all('p') if "Answer:" in item.text]

'''
Turns out in this list, there's one answer that isn't formatted like "Answer: blah blah blah

Figured this out by checking the length of questions and answers (249, 248)

Then i did list(zip(questions[:249],answers)) and compared the questions and answers against what
was on the page. Not that hard to do, it's in order so you just have to see where the answer doesn't
make sense. That's where the issue will be 
'''


'''
It was for the question: What does DC stand for
Detective Comics

Should be in the 69th index of answers, so we can insert it here
'''

answers.insert(69, "Detective Comics")

#Zip the questions and answers together
QA = list(zip(questions,answers))

#Save it to a csv file
pd.DataFrame(QA).to_csv('Questions_and_Answers.csv',index=False)


#Part 2: Get 5 Random Trivia questions and email it to me

#Read csv and turn it into a list of lists.
Q_and_A = pd.read_csv('Questions_and_Answers.csv').values.tolist()

def grouper(iterable,n,fillvalue=None):
    args = [iter(iterable)] * n
    return zip_longest(*args, fillvalue=fillvalue)

QA_Groups = list(grouper(Q_and_A,5))

email = open('your email').read()
password=open('your password').read()

for qas in QA_Groups:
    
    msg = EmailMessage()
    
    msg['Subject'] = "Trivia Time!"
    
    msg['From'] = email
    msg['To'] = email
    
    question_group = '\n\n'.join([item[0] for item in qas])
    answer_group = '\n\n'.join([item[1] for item in qas])
    
    msg.set_content(question_group + '\n'*10 + answer_group)

        
    with smtplib.SMTP_SSL('smtp.gmail.com',465) as smtp:
        smtp.login(email,password)
        smtp.send_message(msg)
    
    time.sleep(1800) #1800 seconds = 30 minutes
