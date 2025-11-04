Linux - Easy

Nmap:

```
nmap -sC -sV 10.10.11.80
```

<img width="1280" height="627" alt="image" src="https://github.com/user-attachments/assets/2713ce34-19ee-4e7b-90da-4591fd64dfc4" />

<img width="1518" height="687" alt="{2EFD0F66-2319-4345-A81A-696E4241DE18}" src="https://github.com/user-attachments/assets/dee8dd78-cc7c-4b59-ac62-e0ae483f4ab3" />

Port 8080 sử dụng CMS Xwiki, chạy trên Jetty server (viết bằng Java):

<img width="1510" height="351" alt="{49E65F93-7F90-429D-8573-34A71E177BCE}" src="https://github.com/user-attachments/assets/70c9a3e1-c479-42f5-afd9-f205355a9193" />

Có phải version 1.1?

<img width="1569" height="381" alt="{6E818D56-D1FE-4EEA-9C76-53A1360B7837}" src="https://github.com/user-attachments/assets/cee5e405-e970-40d6-8a46-856a2b64c086" />

Search luôn Xwiki CVE, thấy 1 unauth RCE: https://www.offsec.com/blog/cve-2025-24893/ của offsec luôn, khá uy tín :)))

Nhận thấy version là 15.10.8

<img width="1641" height="795" alt="{F084FDB0-9006-4549-97DA-C935B104651D}" src="https://github.com/user-attachments/assets/bd49bbd8-dc2a-4266-ba8a-0990da315634" />

Theo blog là dính:

<img width="1259" height="412" alt="{8E0BF635-FBA9-41A1-AA86-779117407C9D}" src="https://github.com/user-attachments/assets/e54d46d3-dbcc-4c74-a13d-55404a9dee7c" />

Thử 1 số poc trên mạng thì https://github.com/dollarboysushil/CVE-2025-24893-XWiki-Unauthenticated-RCE-Exploit-POC/blob/main/CVE-2025-24893-dbs.py ăn:

<img width="1908" height="780" alt="{B0664EFE-B1AB-47D7-89EF-338D6B11BDC1}" src="https://github.com/user-attachments/assets/7c02d808-a312-4726-a14e-ea13055b76ce" />

<img width="928" height="238" alt="{32C60751-A7E5-4ADD-9215-09042A7ADE2B}" src="https://github.com/user-attachments/assets/840d50a9-4c2a-4c7e-b12a-6f2af111c873" />

Stabilizing the shell - ổn định shell:

```
script /dev/null -c /bin/bash
CTRL + Z
stty raw -echo; fg
Then press Enter twice, and then enter:
export TERM=xterm
```

Reverse shell xong không vào được folder user `oliver`?

<img width="909" height="273" alt="{C8486834-8478-4FE0-A048-25D05AACFDBB}" src="https://github.com/user-attachments/assets/792493c6-e14d-41cd-b859-60abf5de40bb" />

Thôi linpeas đã, cũng không có gì đặc sắc.

Đang là `xwiki` user nên không đọc được folder user `oliver`:

<img width="953" height="255" alt="{3FDD4BDC-FC24-48CF-8847-EA2583D4CD73}" src="https://github.com/user-attachments/assets/004787a6-7330-4555-a3ca-14c72a3afdf4" />

No hope, đi đọc lời giải :)))) -> có thể tìm được file password @@

Quay lại linpeas xem kỹ :)))) không có gì, password là file /usr/lib/xwiki/WEB-INF/hibernate.cfg.xml how to đoán???

<img width="1305" height="258" alt="{B69DC023-E1B8-4E74-9762-E2219B92DAAC}" src="https://github.com/user-attachments/assets/53866819-9d12-4d13-b356-854bb871049d" />

<img width="1344" height="652" alt="{565AB073-5ED6-4522-97E9-DE41C81A797F}" src="https://github.com/user-attachments/assets/ff8b3f93-caf3-4aa7-bcdb-53c3c38fc92f" />

Học được lệnh mới để check file có quyền đặc biệt: `find / -user root -perm -4000 -print 2>/dev/null`

<img width="1251" height="542" alt="{9B20F2CE-59DC-4BF4-8683-36F0015A127E}" src="https://github.com/user-attachments/assets/d7481ce0-f0d3-4399-a878-b3c4a1f1f301" />

/opt/netdata/usr/libexec/netdata/plugins.d/ndsudo có thể leo root

https://github.com/AzureADTrent/CVE-2024-32019-POC làm theo là ra :))) ảo diệu
