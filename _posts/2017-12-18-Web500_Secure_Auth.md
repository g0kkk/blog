---
layout: post
title:  "InCTF Web500 Secure Auth"
image: ''
date:   2017-12-18 00:06:31
tags:
- Web Security, CTF, InCTF, Unicorn JS
description: 'Walkthrough of Web 500 InCTF Secure Auth challenge'
categories:
- Web, CTF, Unicorn JS
---

This year InCTF, the first international CTF (organised by <a href="http://bi0s.in/">team bi0s</a>), ended on 17th of December 2017. Congrats to OpenToAll, Dcua and jinmo123 for finishing in the top three positions respectively.

We had around 27 challenges for the CTF from Web, pwn, RE, Mobile, Crypto and Forensics. Since web is my area, I had designed a challenge, `Secure Auth`
which was not just based on Web, but also on RE. The challenge was designed in such a way that to get the flag, you will have to bypass the authentication which is in JS.

About the challenge, you have a welcome screen where in it is written `Site under maintenance`.
<figure class="foto-legenda">
	<img src="{{ "/assets/img/ctfwriteup/welcome_screen1.png"}}" alt="">
</figure>
Going through the source code and cookies doesn't reveal anything. Since there are several tools available to see the directories available, one such tool being the <a href="https://github.com/gokulkrishna01/Webexploiter"> Web exploiter</a> reveals that there is `robots.txt` and upon going through that, we can see a few folders.

```
User-agent: *
Disallow: /cgi-bin
Disallow: /zxcv
```

`/zxcv` takes us to a login page and upon going through a source code, we can see 2 JS being called, `unicorn.js` and `script.js`.

<figure class="foto-legenda">
	<img src="{{ "/assets/img/ctfwriteup/login_page1.png"}}" alt="">
</figure>

`script.js` reveals the following code:

```
var _0x9c2f=['PROT_ALL','location','href','indexOf','/../','.html','Wrong\x20credentials,\x20sorry','#c_submit','click','preventDefault','#cuser','val','slice','split','reverse','map','charCodeAt','toString','join','#cpass','reg_write_i32','X86_REG_EAX','X86_REG_ECX','X86_REG_EDX','mem_map','emu_start','length','reg_read_i32','ARCH_X86','MODE_32','X86_REG_ESI','X86_REG_EBX','X86_REG_EBP'];(function(a,c){var b=function(b){while(--b){a['push'](a['shift']());}};b(++c);}(_0x9c2f,0x193));var _0xf9c2=function(a,c){a=a-0x0;var b=_0x9c2f[a];return b;};$(_0xf9c2('0x0'))[_0xf9c2('0x1')](function(r){r[_0xf9c2('0x2')]();var h=$(_0xf9c2('0x3'))[_0xf9c2('0x4')]();var n=h[_0xf9c2('0x5')](0x0,0x4);n=n[_0xf9c2('0x6')]('')[_0xf9c2('0x7')]()['join']('');n='0x'+n['split']('')[_0xf9c2('0x8')](_0x29995c=>_0x29995c[_0xf9c2('0x9')](0x0)[_0xf9c2('0xa')](0x10))[_0xf9c2('0xb')]('');var m=h[_0xf9c2('0x5')](0x4,0x8);m=m[_0xf9c2('0x6')]('')[_0xf9c2('0x7')]()[_0xf9c2('0xb')]('');m='0x'+m[_0xf9c2('0x6')]('')[_0xf9c2('0x8')](_0x1c7b1c=>_0x1c7b1c['charCodeAt'](0x0)[_0xf9c2('0xa')](0x10))[_0xf9c2('0xb')]('');var f=h[_0xf9c2('0x5')](0x8,0xc);f=f[_0xf9c2('0x6')]('')[_0xf9c2('0x7')]()[_0xf9c2('0xb')]('');f='0x'+f[_0xf9c2('0x6')]('')[_0xf9c2('0x8')](_0x41acdb=>_0x41acdb[_0xf9c2('0x9')](0x0)['toString'](0x10))[_0xf9c2('0xb')]('');var j=h[_0xf9c2('0x5')](0xc,0x10);j=j[_0xf9c2('0x6')]('')['reverse']()[_0xf9c2('0xb')]('');j='0x'+j[_0xf9c2('0x6')]('')[_0xf9c2('0x8')](_0x4186f9=>_0x4186f9[_0xf9c2('0x9')](0x0)[_0xf9c2('0xa')](0x10))[_0xf9c2('0xb')]('');var g=h[_0xf9c2('0x5')](0x10,0x13);g=g[_0xf9c2('0x6')]('')['reverse']()[_0xf9c2('0xb')]('');g='0x'+g['split']('')[_0xf9c2('0x8')](_0x515f47=>_0x515f47[_0xf9c2('0x9')](0x0)['toString'](0x10))[_0xf9c2('0xb')]('');var c=$(_0xf9c2('0xc'))[_0xf9c2('0x4')]();var o=c[_0xf9c2('0x5')](0x0,0x4);o=o['split']('')[_0xf9c2('0x7')]()['join']('');o='0x'+o[_0xf9c2('0x6')]('')['map'](_0x12efcf=>_0x12efcf['charCodeAt'](0x0)[_0xf9c2('0xa')](0x10))[_0xf9c2('0xb')]('');var i=c['slice'](0x4,0x8);i=i['split']('')[_0xf9c2('0x7')]()[_0xf9c2('0xb')]('');i='0x'+i['split']('')[_0xf9c2('0x8')](_0x3b0ae1=>_0x3b0ae1[_0xf9c2('0x9')](0x0)['toString'](0x10))[_0xf9c2('0xb')]('');var e=c[_0xf9c2('0x5')](0x8,0xc);e=e[_0xf9c2('0x6')]('')[_0xf9c2('0x7')]()['join']('');e='0x'+e['split']('')[_0xf9c2('0x8')](_0x40d2e3=>_0x40d2e3['charCodeAt'](0x0)['toString'](0x10))[_0xf9c2('0xb')]('');var k=c[_0xf9c2('0x5')](0xc,0x10);k=k[_0xf9c2('0x6')]('')[_0xf9c2('0x7')]()['join']('');k='0x'+k[_0xf9c2('0x6')]('')[_0xf9c2('0x8')](_0x71ef00=>_0x71ef00[_0xf9c2('0x9')](0x0)[_0xf9c2('0xa')](0x10))[_0xf9c2('0xb')]('');var l=c[_0xf9c2('0x5')](0x10,0x13);l=l[_0xf9c2('0x6')]('')[_0xf9c2('0x7')]()[_0xf9c2('0xb')]('');l='0x'+l[_0xf9c2('0x6')]('')[_0xf9c2('0x8')](_0x335224=>_0x335224[_0xf9c2('0x9')](0x0)[_0xf9c2('0xa')](0x10))['join']('');var a=new uc['Unicorn'](uc['ARCH_X86'],uc['MODE_32']);var b=0x8048000;var d=[0xbf,0x69,0x61,0x6d,0x74,0x39,0xf8,0x75,0x2b,0xbf,0x68,0x65,0x61,0x64,0x39,0xfb,0x75,0x22,0xbf,0x6d,0x69,0x6e,0x69,0x39,0xf9,0x75,0x19,0xbf,0x73,0x74,0x72,0x61,0x39,0xfa,0x75,0x10,0xbf,0x74,0x6f,0x72,0x0,0x39,0xfe,0x75,0x7,0xb8,0x1,0x0,0x0,0x0,0xeb,0x5,0xb8,0x0,0x0,0x0,0x0];a[_0xf9c2('0xd')](uc[_0xf9c2('0xe')],n);a[_0xf9c2('0xd')](uc['X86_REG_EBX'],m);a[_0xf9c2('0xd')](uc[_0xf9c2('0xf')],f);a[_0xf9c2('0xd')](uc[_0xf9c2('0x10')],j);a[_0xf9c2('0xd')](uc['X86_REG_ESI'],g);a[_0xf9c2('0x11')](b,0x1000,uc['PROT_ALL']);a['mem_write'](b,d);a[_0xf9c2('0x12')](b,b+d[_0xf9c2('0x13')]);var p=a[_0xf9c2('0x14')](uc[_0xf9c2('0xe')]);if(p){var a=new uc['Unicorn'](uc[_0xf9c2('0x15')],uc[_0xf9c2('0x16')]);var b=0x8048000;var d=[0x81,0xc6,0x69,0x61,0x6d,0x74,0x81,0xc6,0x7,0x6,0x5,0x4,0x81,0xfe,0xd3,0xd6,0xe0,0xdf,0x75,0x55,0x40,0x81,0xc3,0x68,0x65,0x61,0x64,0x81,0xc3,0x3,0x2,0x1,0x0,0x81,0xfb,0xdd,0xdb,0xdc,0x98,0x75,0x40,0x40,0x81,0xc1,0x6d,0x69,0x6e,0x69,0x81,0xc1,0xf,0xe,0xd,0xc,0x81,0xf9,0xf0,0xdf,0xe0,0xe5,0x75,0x2b,0x40,0x81,0xc2,0x73,0x74,0x72,0x61,0x81,0xc2,0xb,0xa,0x9,0x8,0x81,0xfa,0xdf,0xf1,0x9f,0xe0,0x75,0x16,0x40,0x81,0xc5,0x74,0x6f,0x72,0x0,0x81,0xc5,0x17,0x16,0x15,0x0,0x81,0xfd,0xbb,0xf7,0xcb,0x0,0x75,0x1,0x40];a[_0xf9c2('0xd')](uc[_0xf9c2('0x17')],o);a[_0xf9c2('0xd')](uc[_0xf9c2('0x18')],i);a[_0xf9c2('0xd')](uc[_0xf9c2('0xf')],e);a[_0xf9c2('0xd')](uc[_0xf9c2('0x10')],k);a[_0xf9c2('0xd')](uc[_0xf9c2('0x19')],l);a[_0xf9c2('0x11')](b,0x1000,uc[_0xf9c2('0x1a')]);a['mem_write'](b,d);a[_0xf9c2('0x12')](b,b+d[_0xf9c2('0x13')]);var q=a[_0xf9c2('0x14')](uc['X86_REG_EAX']);if(q==0x5){if(document[_0xf9c2('0x1b')][_0xf9c2('0x1c')][_0xf9c2('0x1d')]('?p=')==-0x1){document['location']=document[_0xf9c2('0x1b')]+_0xf9c2('0x1e')+c+_0xf9c2('0x1f');}}else{alert(_0xf9c2('0x20'));}}else{alert(_0xf9c2('0x20'));}});
```

We can see that the code is obfuscated and little bit of deobfuscation gives us the following code which is used for the authentication:

```
$("#c_submit").click(function(event) {
    event.preventDefault();
    var u = $("#cuser").val();
    var u1 = u.slice(0,4);
    u1 = u1.split("").reverse().join("");
    u1 = "0x" + u1.split("").map((x) => x.charCodeAt(0).toString(16)).join("")
    var u2 = u.slice(4, 8);
    u2 = u2.split("").reverse().join("");
    u2 = "0x" + u2.split("").map((x) => x.charCodeAt(0).toString(16)).join("")
    var u3 = u.slice(8, 12);
    u3 = u3.split("").reverse().join("");
    u3 = "0x" + u3.split("").map((x) => x.charCodeAt(0).toString(16)).join("")
    var u4 = u.slice(12, 16);
    u4 = u4.split("").reverse().join("");
    u4 = "0x" + u4.split("").map((x) => x.charCodeAt(0).toString(16)).join("")
    var u5 = u.slice(16, 19);
    u5 = u5.split("").reverse().join("");
    u5 = "0x" + u5.split("").map((x) => x.charCodeAt(0).toString(16)).join("")

    var p = $("#cpass").val();
    var p1 = p.slice(0,4);
    p1 = p1.split("").reverse().join("");
    p1 = "0x" + p1.split("").map((x) => x.charCodeAt(0).toString(16)).join("")
    var p2 = p.slice(4, 8);
    p2 = p2.split("").reverse().join("");
    p2 = "0x" + p2.split("").map((x) => x.charCodeAt(0).toString(16)).join("")
    var p3 = p.slice(8, 12);
    p3 = p3.split("").reverse().join("");
    p3 = "0x" + p3.split("").map((x) => x.charCodeAt(0).toString(16)).join("")
    var p4 = p.slice(12, 16);
    p4 = p4.split("").reverse().join("");
    p4 = "0x" + p4.split("").map((x) => x.charCodeAt(0).toString(16)).join("")
    var p5 = p.slice(16, 19);
    p5 = p5.split("").reverse().join("");
    p5 = "0x" + p5.split("").map((x) => x.charCodeAt(0).toString(16)).join("")

    var e = new uc.Unicorn(uc.ARCH_X86, uc.MODE_32);
    var addr = 0x8048000;
    var code = [0xbf, 0x69, 0x61, 0x6d, 0x74, 0x39, 0xf8, 0x75, 0x2b, 0xbf, 0x68,
                0x65, 0x61, 0x64, 0x39, 0xfb, 0x75, 0x22, 0xbf, 0x6d, 0x69, 0x6e,
                0x69, 0x39, 0xf9, 0x75, 0x19, 0xbf, 0x73, 0x74, 0x72, 0x61, 0x39,
                0xfa, 0x75, 0x10, 0xbf, 0x74, 0x6f, 0x72, 0x00, 0x39, 0xfe, 0x75,
                0x07, 0xb8, 0x01, 0x00, 0x00, 0x00, 0xeb, 0x05, 0xb8, 0x00, 0x00,
                0x00, 0x00];
    e.reg_write_i32(uc.X86_REG_EAX, u1);
    e.reg_write_i32(uc.X86_REG_EBX, u2);
    e.reg_write_i32(uc.X86_REG_ECX, u3);
    e.reg_write_i32(uc.X86_REG_EDX, u4);
    e.reg_write_i32(uc.X86_REG_ESI, u5);
    e.mem_map(addr, 4096, uc.PROT_ALL);
    e.mem_write(addr, code);
    e.emu_start(addr, addr + code.length);
    var t = e.reg_read_i32(uc.X86_REG_EAX);
    if (t) {
	    var e = new uc.Unicorn(uc.ARCH_X86, uc.MODE_32);
	    var addr = 0x08048000;
	    var code = [0x81, 0xc6, 0x69, 0x61, 0x6d, 0x74, 0x81, 0xc6, 0x07, 0x06,
                    0x05, 0x04, 0x81, 0xfe, 0xd3, 0xd6, 0xe0, 0xdf, 0x75, 0x55,
                    0x40, 0x81, 0xc3, 0x68, 0x65, 0x61, 0x64, 0x81, 0xc3, 0x03,
                    0x02, 0x01, 0x00, 0x81, 0xfb, 0xdd, 0xdb, 0xdc, 0x98, 0x75,
                    0x40, 0x40, 0x81, 0xc1, 0x6d, 0x69, 0x6e, 0x69, 0x81, 0xc1,
                    0x0f, 0x0e, 0x0d, 0x0c, 0x81, 0xf9, 0xf0, 0xdf, 0xe0, 0xe5,
                    0x75, 0x2b, 0x40, 0x81, 0xc2, 0x73, 0x74, 0x72, 0x61, 0x81,
                    0xc2, 0x0b, 0x0a, 0x09, 0x08, 0x81, 0xfa, 0xdf, 0xf1, 0x9f,
                    0xe0, 0x75, 0x16, 0x40, 0x81, 0xc5, 0x74, 0x6f, 0x72, 0x00,
                    0x81, 0xc5, 0x17, 0x16, 0x15, 0x00, 0x81, 0xfd, 0xbb, 0xf7,
                    0xcb, 0x00, 0x75, 0x01, 0x40];
	    e.reg_write_i32(uc.X86_REG_ESI, p1);
	    e.reg_write_i32(uc.X86_REG_EBX, p2);
	    e.reg_write_i32(uc.X86_REG_ECX, p3);
	    e.reg_write_i32(uc.X86_REG_EDX, p4);
	    e.reg_write_i32(uc.X86_REG_EBP, p5);
	    e.mem_map(addr, 4096, uc.PROT_ALL);
	    e.mem_write(addr, code);
	    e.emu_start(addr, addr + code.length);
	    var check = e.reg_read_i32(uc.X86_REG_EAX);
        if (check == 5) {
            if (document.location.href.indexOf("?p=") == -1) {
                document.location = document.location + "/../" + p + ".html";
            }
        }
        else{
            alert('Wrong credentials, sorry');
        }
    }
    else{
        alert('Wrong credentials, sorry');
    }
});
```
Here, we can see that the username and the password are sliced in little endian and then being stored in registers and the input length being `0x13`. As the description of the challenge says, the username and password has same length. The authentication uses Unicorn which emulates opcodes in browser. The above code has two checks done of which one is for username and the other being for password.

The assembly instructions for the `username` is as follows:

```
      bf 69 61 6d 74          mov    edi,0x746d6169
      39 f8                   cmp    eax,edi
      75 2b                   jne    0x34
      bf 68 65 61 64          mov    edi,0x64616568
      39 fb                   cmp    ebx,edi
      75 22                   jne    0x34
      bf 6d 69 6e 69          mov    edi,0x696e696d
      39 f9                   cmp    ecx,edi
      75 19                   jne    0x34
      bf 73 74 72 61          mov    edi,0x61727473
      39 fa                   cmp    edx,edi
      75 10                   jne    0x34
      bf 74 6f 72 00          mov    edi,0x726f74
      39 fe                   cmp    esi,edi
      75 07                   jne    0x34
      b8 01 00 00 00          mov    eax,0x1
      eb 05                   jmp    0x39
      b8 00 00 00 00          mov    eax,0x0
  ```
and for the `password` is:

```
      81 c6 69 61 6d 74       add    esi,0x746d6169
      81 c6 07 06 05 04       add    esi,0x4050607
      81 fe d3 d6 e0 df       cmp    esi,0xdfe0d6d3
      75 55                   jne    0x69
      40                      inc    eax
      81 c3 68 65 61 64       add    ebx,0x64616568
      81 c3 03 02 01 00       add    ebx,0x10203
      81 fb dd db dc 98       cmp    ebx,0x98dcdbdd
      75 40                   jne    0x69
      40                      inc    eax
      81 c1 6d 69 6e 69       add    ecx,0x696e696d
      81 c1 0f 0e 0d 0c       add    ecx,0xc0d0e0f
      81 f9 f0 df e0 e5       cmp    ecx,0xe5e0dff0
      75 2b                   jne    0x69
      40                      inc    eax
      81 c2 73 74 72 61       add    edx,0x61727473
      81 c2 0b 0a 09 08       add    edx,0x8090a0b
      81 fa df f1 9f e0       cmp    edx,0xe09ff1df
      75 16                   jne    0x69
      40                      inc    eax
      81 c5 74 6f 72 00       add    ebp,0x726f74
      81 c5 17 16 15 00       add    ebp,0x151617
      81 fd bb f7 cb 00       cmp    ebp,0xcbf7bb
      75 01                   jne    0x69
      40                      inc    eax
```

We can get the `username` as `iamtheadministrator` and `password` as `congrtz4thepas$w0rD` from the above respective codes through which we can get past the login page. Upon successful authentication, we get redirected to the `ChallengeURL/congrtz4thepas$w0rD.html`. From the source code, we can get the flag `inctf{l1ck_l1ck_l1ck_my_j5}`.
