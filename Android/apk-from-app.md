Lấy tên package, check path và pull về:

```
pm list pakage | grep <...>
pm path <package_name>
```

<img width="1880" height="446" alt="image" src="https://github.com/user-attachments/assets/30b1a602-34c4-4dab-a25e-339f4c5c745f" />

Trường hợp này ứng dụng sử dụng Android App Bundles nên có các các split apks (bao gồm base.apk và các split_config.*.apk). Muốn install lại cần dùng adb install-multiple thay vì install bình thường:

<img width="1901" height="498" alt="image" src="https://github.com/user-attachments/assets/03582644-db09-47eb-b240-dff1fb209453" />

Khi phân tích app bằng jadx, base.apk chứa code java, kotlin, … các lib .so nằm trong file `split_config.<structure>.apk`
