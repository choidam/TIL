## picker 사용하기

1. 전체를 잡는 view 생성 + constraint 걸기

```swift
   self.picker.isHidden = false
   self.picker.frame = CGRect(x: 0, y: view.frame.height - 220, width: view.frame.width, height: 150)
   view.addSubview(self.picker)
```

<br/>
2. Tool Bar 에 들어갈 버튼 생성

```swift
   let btnDone = UIBarButtonItem(barButtonSystemItem: .done, target: self, action: #selector(self.testfunc))
```


<br/>

3. UIPicker Toolbar 생성 + 속성 setting
        
```swift
   let barAccessory = UIToolbar(frame: CGRect(x: 0, y: 0, width: picker.frame.width, height: 44))
        
   barAccessory.barStyle = UIBarStyle.default
   barAccessory.isTranslucent = true
   barAccessory.items = [btnDone]
        
   picker.addSubview(barAccessory)
```

<br/>
4. UIPickerView 생성

```swift
   self.pickerView.frame = CGRect(x: 0, y: barAccessory.frame.height, width: view.frame.width, height: picker.frame.height - barAccessory.frame.height)
        
   self.pickerView.delegate = self
   self.pickerView.dataSource = self
   self.pickerView.backgroundColor = UIColor.whiteFour
        
   picker.addSubview(self.pickerView)
```
