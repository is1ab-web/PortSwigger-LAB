# PortSwigger-LAB

## 🔎 Index

- [SQL Injection](#sql-injection)
- [Cross-site Scripting (XSS)](#cross-site-scripting)
- [CSRF](#csrf)
- [Clickjacking](#clickjacking)
- [DOM-based Vulnerabilities](#dom-based-vulnerabilities)
- [CORS](#cors)
- [XXE](#xml-external-entity-xxe-injection)
- [SSRF](#ssrf)
- [HTTP Request Smuggling](#http-request-smuggling)
- [OS Command Injection](#os-command-injection)
- [SSTI](#server-side-template-injection)
- [Path Traversal](#file-path-traversal)
- [Access Control](#access-control-vulnerabilities)
- [Authentication](#authentication-vulnerabilities)
- [WebSockets](#websockets-vulnerabilities)
- [Insecure Deserialization](#insecure-deserialization)
- [Information Disclosure](#information-disclosure)
- [Business Logic](#business-logic-vulnerabilities)
- [JWT](#jwt)
- [NoSQL Injection](#nosql-injection)
- [API](#api)
- [Web LLM](#web-llm)

### SQL injection

<details>
<summary>📂 Labs</summary> 
    
### SQL injection vulnerability in WHERE clause allowing retrieval of hidden data -> [write up](https://hackmd.io/@mio0813/HyDstWVXgx)

### SQL injection vulnerability in WHERE clause allowing retrieval of hidden data -> [write up](https://hackmd.io/@mio0813/B1SphWVQll)

### SQL SQL injection attack, querying the database type and version on Oracle -> [write up](https://hackmd.io/@mio0813/B14Ra-47xg)

### SQL injection attack, querying the database type and version on MySQL and Microsoft -> [write up](https://hackmd.io/@mio0813/S14mWGE7gl)

### SQL injection attack, listing the database contents on non-Oracle databases -> [write up](https://hackmd.io/@mio0813/H1bcXG4Xgl)

### SQL injection attack, listing the database contents on Oracle -> [write up](https://hackmd.io/@mio0813/S1bSFMNQee)

### SQL injection UNION attack, determining the number of columns returned by the query -> [write up](https://hackmd.io/@mio0813/HJ-UYMNmlx)

### SQL injection UNION attack, finding a column containing text -> [write up](https://hackmd.io/@mio0813/r11-JmVQex)

### SQL injection UNION attack, retrieving data from other tables -> [write up](https://hackmd.io/@mio0813/SkasZX47le)

### SQL injection UNION attack, retrieving multiple values in a single column -> [write up](https://hackmd.io/@mio0813/Bkghn7V7lg)

### Blind SQL injection with conditional responses -> [write up](https://hackmd.io/@mio0813/SJICGPnNee)

### Blind SQL injection with conditional errors -> [write up](https://hackmd.io/@mio0813/rkNQl_hVlg)

### Visible error-based SQL injection -> [write up](https://hackmd.io/@mio0813/B1ofeY3Vll)

### Blind SQL injection with time delays -> [write up](https://hackmd.io/@mio0813/By6IB92Exx)

### Blind SQL injection with time delays and information retrieval -> [write up](https://hackmd.io/@mio0813/B1T99chExg)

### Blind SQL injection with out-of-band interaction -> [write up](https://hackmd.io/@mio0813/BJOmXj3Vex)

### Blind SQL injection with out-of-band data exfiltration -> [write up](https://hackmd.io/@mio0813/SyoyMIpVex)

### SQL injection with filter bypass via XML encoding-> [write up](https://hackmd.io/@mio0813/HyHJBLpExl)
</details>

### Cross-site scripting

<details>
<summary>📂 Labs</summary> 

### Cross-site scripting Lab1 -> [write up](https://hackmd.io/@mio0813/B1OMdUGreg)

### Cross-site scripting Lab2 -> [write up](https://hackmd.io/@mio0813/HJxTOUzSgg)

### Cross-site scripting Lab3 -> [write up](https://hackmd.io/@mio0813/SyKCK8MSex)

### Cross-site scripting Lab4 -> [write up](https://hackmd.io/@mio0813/Bk4tj8MHle)

### Cross-site scripting Lab5 -> [write up](https://hackmd.io/@mio0813/HkS_aIGrxg)

### Cross-site scripting Lab6 -> [write up](https://hackmd.io/@mio0813/SkBJWvzHxg)

### Cross-site scripting Lab7 -> [write up](https://hackmd.io/@mio0813/Bk6MVvfSxx)

### Cross-site scripting Lab8 -> [write up](https://hackmd.io/@mio0813/BkCBBwMBgg)

### Cross-site scripting Lab9 -> [write up](https://hackmd.io/@mio0813/r1v9uPMrgx)

### Cross-site scripting Lab10 -> [write up](https://hackmd.io/@mio0813/HkxPswfSxe)

### Cross-site scripting Lab11 -> [write up](https://hackmd.io/@mio0813/Hkdh0PzHgl)

### Cross-site scripting Lab12 -> [write up](https://hackmd.io/@mio0813/S1mCedfHxg)

### Cross-site scripting Lab13 -> [write up](https://hackmd.io/@mio0813/SkcGSOGrgl)

### Cross-site scripting Lab14 -> [write up](https://hackmd.io/@mio0813/HyFbvdfBlg)

### Cross-site scripting Lab15 -> [write up](https://hackmd.io/@mio0813/BkJh5_fBel)

### Cross-site scripting Lab16 -> [write up](https://hackmd.io/@mio0813/S13JvgXBeg)

### Cross-site scripting Lab17 -> [write up](https://hackmd.io/@mio0813/H1e8jgmrxl)

### Cross-site scripting Lab18 -> [write up](https://hackmd.io/@mio0813/HkFlJWQreg)

### Cross-site scripting Lab19 -> [write up](https://hackmd.io/@mio0813/rksNbbQSeg)

### Cross-site scripting Lab20 -> [write up](https://hackmd.io/@mio0813/S1XQXW7rll)

### Cross-site scripting Lab21 -> [write up](https://hackmd.io/@mio0813/SkfGLbXHel)

### Cross-site scripting Lab22 -> [write up](https://hackmd.io/@mio0813/S1FquZXrxg)

### Cross-site scripting Lab23 -> [write up](https://hackmd.io/@mio0813/rk7G0bmBll)

### Cross-site scripting Lab24 -> [write up](https://hackmd.io/@mio0813/rkDh4MQHll)

### Cross-site scripting Lab25 -> [write up](https://hackmd.io/@mio0813/HJD8PGmrll)

### Cross-site scripting Lab26 -> [write up](https://hackmd.io/@mio0813/S1USYz7rlg)

### Cross-site scripting Lab27 -> [write up](https://hackmd.io/@mio0813/HJw8sMQSgx)

### Cross-site scripting Lab28 -> [write up](https://hackmd.io/@mio0813/BJLY6MXHgg)

### Cross-site scripting Lab29 -> [write up](https://hackmd.io/@mio0813/BktXWQQree)

### Cross-site scripting Lab30 -> [write up](https://hackmd.io/@mio0813/rkwaLQmBgx)

</details>
    
### CSRF

<details>
<summary>📂 Labs</summary> 
    
### Web CSRF Lab1 -> [write up](https://hackmd.io/@mio0813/BkAPBHu-gg)

### Web CSRF Lab2 -> [write up](https://hackmd.io/@mio0813/BJ0wgpK-ll)

### Web CSRF Lab3 -> [write up](https://hackmd.io/@mio0813/rkAdh9p-gg)

### Web CSRFLab4 -> [write up](https://hackmd.io/@mio0813/rJgdtCzMxg)

</details>
    
### Clickjacking

<details>
<summary>📂 Labs</summary> 
    
### Clickjacking Lab1 -> [write up](https://hackmd.io/@mio0813/HkN7hR6Vlg)

### Clickjacking Lab2 -> [write up](https://hackmd.io/@mio0813/ryZE-JC4xl)

### Clickjacking Lab3 -> [write up](https://hackmd.io/@mio0813/H1Tld1CNee)

### Clickjacking Lab4 -> [write up](https://hackmd.io/@mio0813/SJEmcyRVgg)

### Clickjacking Lab5 -> [write up](https://hackmd.io/@mio0813/ry43Se0Nxl)

</details>
        
### DOM-based vulnerabilities

<details>
<summary>📂 Labs</summary> 
    
### DOM-based vulnerabilities Lab1 -> [write up](https://hackmd.io/@mio0813/BypTRgRVel)

### DOM-based vulnerabilities Lab2 -> [write up](https://hackmd.io/@mio0813/HyPiObCVeg)

### DOM-based vulnerabilities Lab3 -> [write up](https://hackmd.io/@mio0813/SybHcWAEle)

### DOM-based vulnerabilities Lab4 -> [write up](https://hackmd.io/@mio0813/BJoqTbAEeg)

### DOM-based vulnerabilities Lab5 -> [write up](https://hackmd.io/@mio0813/r11R-X0Nge)

### DOM-based vulnerabilities Lab6-> [write up](https://hackmd.io/@mio0813/ryUbLXRNxg)

### DOM-based vulnerabilities Lab7 -> [write up](https://hackmd.io/@mio0813/r1RDFX04ll)

</details>
        
### CORS

<details>
<summary>📂 Labs</summary> 
    
### CORS Lab1 -> [write up](https://hackmd.io/@mio0813/HJtdhQXBex)

### CORS Lab2 -> [write up](https://hackmd.io/@mio0813/By5gxV7Hee)

### CORS Lab3 -> [write up](https://hackmd.io/@mio0813/rkAtQNXSlg)

</details>
        
### XML external entity (XXE) injection

<details>
<summary>📂 Labs</summary> 
    
### XXE Lab1 -> [write up](https://hackmd.io/@mio0813/HyjRLNXHll)

### XXE Lab2 -> [write up](https://hackmd.io/@mio0813/HkCsFN7rll)

### XXE Lab3 -> [write up](https://hackmd.io/@mio0813/B1Rl6NXHgx)

### XXE Lab4 -> [write up](https://hackmd.io/@mio0813/BJlaCEQBxl)

### XXE Lab5 -> [write up](https://hackmd.io/@mio0813/BJZIlH7Hxe)

### XXE Lab6 -> [write up](https://hackmd.io/@mio0813/BJNyNH7See)

### XXE Lab7 -> [write up](https://hackmd.io/@mio0813/BJyrSr7Hgx)

### XXE Lab8 -> [write up](https://hackmd.io/@mio0813/BJ1tLBmHxg)

### XXE Lab9 -> [write up](https://hackmd.io/@mio0813/H1823B7Hxl)

</details>
        
### SSRF

<details>
<summary>📂 Labs</summary> 
    
### SSRF Lab1 -> [write up](https://hackmd.io/@mio0813/HkROgLmBlg)

### SSRF Lab2 -> [write up](https://hackmd.io/@mio0813/r171zLQSlx)

### SSRF Lab3 -> [write up](https://hackmd.io/@mio0813/BJeneNL7Hxg)

### SSRF Lab4 -> [write up](https://hackmd.io/@mio0813/BylQHL7Bgg)

### SSRF Lab5 -> [write up](https://hackmd.io/@mio0813/HJ_68Lmree)

### SSRF Lab6 -> [write up](https://hackmd.io/@mio0813/rkePA9LQBxg)

### SSRF Lab7 -> [write up](https://hackmd.io/@mio0813/SyLdRLQSgx)

</details>
        
### HTTP request smuggling

<details>
<summary>📂 Labs</summary> 
    
### HTTP request smuggling Lab1 -> [write up](https://hackmd.io/@mio0813/SkVqzP7Hxx)

### HTTP request smuggling Lab2 -> [write up](https://hackmd.io/@mio0813/SyehnzmPxe)

### HTTP request smuggling Lab3 -> [write up](https://hackmd.io/@mio0813/S1QjDzXDel)

### HTTP request smuggling Lab4 -> [write up](https://hackmd.io/@mio0813/ByO2X7mDgl)

### HTTP request smuggling Lab5 -> [write up](https://hackmd.io/@mio0813/Hk3aKQLDxe)

### HTTP request smuggling Lab6 -> [write up](https://hackmd.io/@mio0813/rkRTyELPex)

### HTTP request smuggling Lab7 -> [write up](https://hackmd.io/@mio0813/ryZmlBUwgl)

### HTTP request smuggling Lab8 -> [write up](https://hackmd.io/@mio0813/HktDSB8vxl)

### HTTP request smuggling Lab9 -> [write up](https://hackmd.io/@mio0813/S19a2BIvge)

### HTTP request smuggling Lab10 -> [write up](https://hackmd.io/@mio0813/BkULRuLPxg)

### HTTP request smuggling Lab11 -> [write up](https://hackmd.io/@mio0813/r13oTY8Plg)

</details>
        
### OS command injection

<details>
<summary>📂 Labs</summary> 
    
### OS command injection Lab1 -> [write up](https://hackmd.io/@mio0813/rJyfmNA4xe)

### OS command injection Lab2 -> [write up](https://hackmd.io/@mio0813/SJCyrV0Eex)

### OS command injection Lab3 -> [write up](https://hackmd.io/@mio0813/S1HBDNCElg)

### OS command injection Lab4 -> [write up](https://hackmd.io/@mio0813/SyFT9VCVxe)

### OS command injection Lab -> [write up](https://hackmd.io/@mio0813/rkZlaEAVlx)

</details>
        
### Server-side template injection

<details>
<summary>📂 Labs</summary> 
    
### Server-side template injection Lab1 -> [write up](https://hackmd.io/@mio0813/HJbm4Dmrlx)

### Server-side template injection Lab2 -> [write up](https://hackmd.io/@mio0813/SytEf5IPgx)

### Server-side template injection Lab3 -> [write up](https://hackmd.io/@mio0813/Hka2a9Lvxg)

### Server-side template injection Lab4 -> [write up](https://hackmd.io/@mio0813/B1z5t26Nle)

### Server-side template injection Lab5 -> [write up](https://hackmd.io/@mio0813/HJijmoLvee)

### Server-side template injection Lab6 -> [write up](https://hackmd.io/@mio0813/SJxC8iUvex)

### Server-side template injection Lab7 -> [write up](https://hackmd.io/@mio0813/ryNsco8Dlg)

</details>
        
### File path traversal

<details>
<summary>📂 Labs</summary> 
    
### File path traversal Lab -> [write up](https://hackmd.io/@mio0813/r1MjIk6keg)

### File path traversal Lab2 -> [write up](https://hackmd.io/@mio0813/BJzgqJ6kle)

### File path traversal Lab3 -> [write up](https://hackmd.io/@mio0813/B1Vrsk6yge)

### File path traversal Lab4 -> [write up](https://hackmd.io/@mio0813/BJkmcEE7le)

### File path traversal Lab5 -> [write up](https://hackmd.io/@mio0813/BJSoAN4Qel)

### File path traversal Lab6 -> [write up](https://hackmd.io/@mio0813/r1-jVSVQxx)

</details>
        
### Access control vulnerabilities

<details>
<summary>📂 Labs</summary> 
    
### Access control vulnerabilities Lab1 -> [write up](https://hackmd.io/@mio0813/HJSiEPXHxg)

### Access control vulnerabilities Lab2 -> [write up](https://hackmd.io/@mio0813/SJndSDmHll)

### Access control vulnerabilities Lab3 -> [write up](https://hackmd.io/@mio0813/rkveIv7Sel)

### Access control vulnerabilities Lab4 -> [write up](https://hackmd.io/@mio0813/HyCJDvXBgg)

### Access control vulnerabilities Lab5 -> [write up](https://hackmd.io/@mio0813/Byk6uvQSel)

### Access control vulnerabilities Lab6 -> [write up](https://hackmd.io/@mio0813/rJBOKwXreg)

### Access control vulnerabilities Lab7 -> [write up](https://hackmd.io/@mio0813/BJpv9Pmrel)

### Access control vulnerabilities Lab8 -> [write up](https://hackmd.io/@mio0813/rk7zhwQrll)

### Access control vulnerabilities Lab9 -> [write up](https://hackmd.io/@mio0813/Hy1MpwQHeg)

### Access control vulnerabilities Lab10 -> [write up](https://hackmd.io/@mio0813/BkvVy_QHxg)

### Access control vulnerabilities Lab11 -> [write up](https://hackmd.io/@mio0813/S1CTZd7Sxe)

### Access control vulnerabilities Lab12 -> [write up](https://hackmd.io/@mio0813/rJHDw_mSee)

</details>
        
### Authentication vulnerabilities

<details>
<summary>📂 Labs</summary> 
    
### Authentication vulnerabilities Lab1 -> [write up](https://hackmd.io/@mio0813/BJPq02Uvgx)

### Authentication vulnerabilities Lab2 -> [write up](https://hackmd.io/@mio0813/HkvueaIDgl)

### Authentication vulnerabilities Lab3 -> [write up](https://hackmd.io/@mio0813/SkSPMa8Dlx)

### Authentication vulnerabilities Lab4 -> [write up](https://hackmd.io/@mio0813/BJTkrpUPxx)

### Authentication vulnerabilities Lab5 -> [write up](https://hackmd.io/@mio0813/Byq4DTLvxx)

### Authentication vulnerabilities Lab6 -> [write up](https://hackmd.io/@mio0813/H1Gv1AIDll)

### Authentication vulnerabilities Lab7 -> [write up](https://hackmd.io/@mio0813/H1XxQ08wgl)

### Authentication vulnerabilities Lab8 -> [write up](https://hackmd.io/@mio0813/HylAGJwPxe)

### Authentication vulnerabilities Lab9 -> [write up](https://hackmd.io/@mio0813/rk9YukwDgg)

### Authentication vulnerabilities Lab10 -> [write up](https://hackmd.io/@mio0813/S1NegeDwee)

### Authentication vulnerabilities Lab11 -> [write up](https://hackmd.io/@mio0813/By-oVlwDle)

### Authentication vulnerabilities Lab12 -> [write up](https://hackmd.io/@mio0813/SkAhqlvvge)

### Authentication vulnerabilities Lab13 -> [write up](https://hackmd.io/@mio0813/ryDD0xwDll)

### Authentication vulnerabilities Lab14 -> [write up](https://hackmd.io/@mio0813/HyOECstPxg)

</details>
    
### WebSockets vulnerabilities

<details>
<summary>📂 Labs</summary> 
    
### WebSockets vulnerabilities Lab1 -> [write up](https://hackmd.io/@mio0813/B1Pzw8tQxl)

### WebSockets vulnerabilities Lab2 -> [write up](https://hackmd.io/@mio0813/B1EfhLKmeg)

### WebSockets vulnerabilities Lab3 -> [write up](https://hackmd.io/@mio0813/r15V5wYQex)

</details>
        
### Insecure deserialization

<details>
<summary>📂 Labs</summary> 
    
### Insecure deserialization Lab1 -> [write up](https://hackmd.io/@mio0813/ByPmM6tvgl)

### Insecure deserialization Lab2 -> [write up](https://hackmd.io/@mio0813/HktyV6Fvgx)

### Insecure deserialization Lab3 -> [write up](https://hackmd.io/@mio0813/BJN5U6KDle)

### Insecure deserialization Lab4 -> [write up](https://hackmd.io/@mio0813/HJkOJ0Fveg)

### Insecure deserialization Lab5 -> [write up](https://hackmd.io/@mio0813/S1gy7CYPeg)

### Insecure deserialization Lab6 -> [write up](https://hackmd.io/@mio0813/rk7ANcoPex)

### Insecure deserialization Lab7 -> [write up](https://hackmd.io/@mio0813/Sk00Vjswxe)

### Insecure deserialization Lab8 -> [write up](https://hackmd.io/@mio0813/rJp5xhjDge)

### Insecure deserialization Lab9 -> [write up](https://hackmd.io/@mio0813/S1oSlenDee)

### Insecure deserialization Lab10 -> [write up](https://hackmd.io/@mio0813/rJSimlnvlg)

</details>
        
### Information disclosure

<details>
<summary>📂 Labs</summary> 
    
### Information disclosure Lab1 -> [write up](https://hackmd.io/@mio0813/B1xhStREge)

### Information disclosure Lab2 -> [write up](https://hackmd.io/@mio0813/HyVpFYAEex)

### Information disclosure Lab3 -> [write up](https://hackmd.io/@mio0813/H1gAN0NzBgl)

### Information disclosure Lab4 -> [write up](https://hackmd.io/@mio0813/HJDTlSzSlg)

### Information disclosure Lab5 -> [write up](https://hackmd.io/@mio0813/H1GSCVzHgl)

</details>
        
### Business logic vulnerabilities

<details>
<summary>📂 Labs</summary>
    
### Business logic vulnerabilities Lab1 -> [write up](https://hackmd.io/@mio0813/HyVPf-2wll)

### Business logic vulnerabilities Lab2 -> [write up](https://hackmd.io/@mio0813/S10pSEhwgl)

### Business logic vulnerabilities Lab3 -> [write up](https://hackmd.io/@mio0813/SkaK5E3Del)

### Business logic vulnerabilities Lab4 -> [write up](https://hackmd.io/@mio0813/HJCbpN2wxl)

### Business logic vulnerabilities Lab5 -> [write up](https://hackmd.io/@mio0813/SJ2jlS2wgx)

### Business logic vulnerabilities Lab6 -> [write up](https://hackmd.io/@mio0813/BkTBDS2Dgg)

### Business logic vulnerabilities Lab7 -> [write up](https://hackmd.io/@mio0813/BkY25rhPll)

### Business logic vulnerabilities Lab8 -> [write up](https://hackmd.io/@mio0813/SJkoaS2weg)

### Business logic vulnerabilities Lab9 -> [write up](https://hackmd.io/@mio0813/r1vnJL2Pgl)

### Business logic vulnerabilities Lab10 -> [write up](https://hackmd.io/@mio0813/HkleXLnwxe)

### Business logic vulnerabilities Lab11 -> [write up](https://hackmd.io/@mio0813/r1nMcU3vxl)

</details>
        
### JWT

<details>
<summary>📂 Labs</summary>
    
### JWT Lab1 -> [write up](https://hackmd.io/@mio0813/SkXlXw2Pgx)

### JWT Lab2 -> [write up](https://hackmd.io/@mio0813/HyHwcqhPeg)

</details>
        
### NoSQL injection

<details>
<summary>📂 Labs</summary>
    
### NoSQL injection Lab1 -> [write up](https://hackmd.io/@mio0813/HyfRbUYXxl)

### NoSQL injection Lab2 -> [write up](https://hackmd.io/@mio0813/r1VOuop4xe)

### NoSQL injection Lab3 -> [write up](https://hackmd.io/@mio0813/By9wk3pNxl)

### NoSQL injection Lab4 -> [write up](https://hackmd.io/@mio0813/B1z5t26Nle)

</details>
        
### API

<details>
<summary>📂 Labs</summary>
    
### API Lab1 -> [write up](https://hackmd.io/@mio0813/Sy3H6D1yxg)

### API Lab2 -> [write up](https://hackmd.io/@mio0813/Sy3H6D1yxg)

### API Lab3 -> [write up](https://hackmd.io/@mio0813/Sy3H6D1yxg)

### API Lab4 -> [write up](https://hackmd.io/@mio0813/Sy3H6D1yxg)

</details>
        
### Web LLM

<details>
<summary>📂 Labs</summary>
    
### Web LLM Lab1 -> [write up](https://hackmd.io/@mio0813/ryAZBVvJgg)

### Web LLM Lab2 -> [write up](https://hackmd.io/@mio0813/HJjERLDkeg)

### Web LLM Lab3 -> [write up](https://hackmd.io/@mio0813/r1jSgKwJee)

### Web LLM La4 -> [write up](https://hackmd.io/@mio0813/r1jSgKwJee)

</details>
        
---
