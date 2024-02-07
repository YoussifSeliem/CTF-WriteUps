# Missing Person

Link: https://cybertalents.com/challenges/osci/missing-person

### Challenge Description
```
Dear agent

Winwodem Strulovitch has been missing, we need your help to find him. Your first task is to find the time of his last possible activity on the internet.

---

Notes:

1. Check the attached document which contains some information we got from his family about him.

2. The timezone of your answer must be UTC and the time format must be 24-hour format.

3. Provide your answer using the following format:
flag{dd-mm-yyyy_hh:mm}
```

### solution

- First start by openning the link attached with the challange
- we find this

<img src=./images/Capture.PNG>

- There a link for a tweet let's try to access this link
- This bit.ly link forwards me to a post in X (Twitter) but the post cann't be viewed
- anyway we can make use of bit.ly by appending `+` to the url this will move us to this site

<img src=./images/Capture1.PNG>

- There's a date right there and this's the flag
- congratzzzzzzzzzzzzzzz


#### beyond the challange

- we may be more lucky to find it this fast but there's other thing you may think about
- as the bit.ly forwords us to post on X (Twitter), so we knew his account name
- This may move us to use a tool called `sherlock` to know if there's an account with the same name on other sites
- This may be useful in other challanges
- Good luck ;)