# Cross-Site Scripting (XSS)

XSS is a web applications vulnerability that occur when an attacker injects scripts into web pages viewed by other users. These scripts execute within the context of the victim's browser, allowing the attacker to inquire information or manipulate web page content.

The following contains explanation of some types of XSS and some XSS related files made by me to practice the concept. These files can be found in `Practice` folder.

# Types of XSS

There are three types of XSS.

- ### Reflected XSS

The malicious script is reflected on the webpage in search results or error messages.

- ### Stored XSS

The script is stored on the web server. For example, the XSS payload is inside username or comments which is executed when other users view them.

- ### DOM-based XSS

The client-side JavaScript modifies the Document Object Model (DOM) in an unsafe way, allowing the attacker to manipulate the DOM to execute malicious code.

# Links for learning more

- ### Readings
    
    - [Cross-site scripting on wikipedia](https://en.wikipedia.org/wiki/Cross-site_scripting)

    - [Cross Site Scripting (XSS) OWASP](https://owasp.org/www-community/attacks/xss/)

    - [What is Cross Site Scripting - CTF Handbook](https://ctf101.org/web-exploitation/cross-site-scripting/what-is-cross-site-scripting/)

- ### Videos

    - [What is PHP and why is XSS so common there? - web 0x02 - LiveOverflow](https://www.youtube.com/watch?v=Q2mGcbkX550)

    - [DO NOT USE alert(1) for XSS - LiveOverflow](https://www.youtube.com/watch?v=KHwVjzWei1c)

    - [The Origin of Cross-Site Scripting (XSS) - Hacker Etymology - LiveOverflow](https://www.youtube.com/watch?v=mKAWpFdVcPY)

- ### Challenges

    - [XSS Game](http://www.xssgame.com/)

# Practice

- ### Alert

Simple XSS practice. Go to `Practice/Alert` folder and open the `index.html` file in the browser. Then, try to pop an alert.

### Notes

[Developer tool](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Tools_and_setup/What_are_browser_developer_tools) can be used to check how `<div id="welcome-text">` is being updated for every submitted input.

- ### Cookies

`Practice/Cookies`. This is very similar to previous practice but instead of popping an alert, the goal of this practice is to create a http server and then, try to read cookie from the webpage and send the information to the server only with XSS.

### Notes

We can create a quick http server with python. `py -m http.server 8000`

[https://webhook.site](https://webhook.site) is a great tool for listening http requests. 