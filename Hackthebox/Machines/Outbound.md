Linux - Easy

Description cho 1 account:

```
As is common in real life pentests, you will start the Outbound box with credentials for the following account tyler / LhKL1o9Nm3X2
```

Scan nmap ra 2 port 80 và 22, cứ tưởng là account SSH nhưng không phải. Truy cập trực tiếp IP thấy browser hiện host `mail.outbound.htb`, set host xong truy cập được dịch vụ http, dùng Roundcube Webmail:

<img width="1282" height="376" alt="{5F4D4E60-AC27-4A82-8405-9C39926B6F52}" src="https://github.com/user-attachments/assets/db6d393c-e7ef-4847-9460-1852c845a29b" />

<img width="1696" height="584" alt="{89CD221D-F4D2-4C2D-AB79-EB860485CFE2}" src="https://github.com/user-attachments/assets/ec314209-88e7-4c13-bf93-f1dce2768b10" />

Account dùng để login webmail:

<img width="1907" height="831" alt="{10026085-FE64-46CB-8733-27FF3E3773E1}" src="https://github.com/user-attachments/assets/9e157db0-b84b-4645-8011-9b6293a42afa" />

Check CVE của Roundcube thấy blog https://www.offsec.com/blog/cve-2025-49113/ là post-auth, machine cũng cung cấp acc, khả năng cao là con này, nhưng chưa kiếm được version ở đâu để confirm. Thử search POC luôn: https://github.com/hakaioffsec/CVE-2025-49113-exploit

<img width="1545" height="240" alt="{F0B02668-54B5-4869-8C40-68C1144E7000}" src="https://github.com/user-attachments/assets/118ae6e9-5167-46d2-bb08-f94b0cdba327" />

<img width="1906" height="377" alt="{B4CB35C2-01C4-4D48-BFED-FC69BFE812FD}" src="https://github.com/user-attachments/assets/2affca75-9949-434b-8662-a8d8ff0484c0" />

Hóa ra 10610 chính là version, chắc là 1.6.10? Check trong index.php có:

<img width="1595" height="678" alt="{972E2A48-D86B-4704-B7AB-62B7E83ECC4E}" src="https://github.com/user-attachments/assets/6dfed52b-1205-4cf7-8391-26e28de01721" />

Thử các kiểu reverse shell không được, dùng msfconsole được, lệnh chạy lỗi hết, lý do shell không reverse được:

Sử dụng password của tyler `su tyler` switch sang tyler.
