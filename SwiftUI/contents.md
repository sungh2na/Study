### Text, Image
- resizable() 
    - 이거 사용해야 프레임으로 크기 지정 가능
- frame()
    - 크기 지정
- edgesIgnoringSafeArea(.all)
    - sageArea 무시
- aspectRatio(contentMode: .fit)
- aspectRatio(contentMode: .fill)

- VStack(spacing: 0)
    - 간격 없애기, 텍스트는 없는데 이미지는 간격이 어느정도 있음

- padding(.bottom, 20)
    - 간격 띄우기

- mask( Circle())
    - 동그랗게


### SFSymbols 
- Image(systemName: "wifi") 
    -  SFsymbols에서 이미지 이름 찾아서 쓰기

### Button
- State (Property wrapper)
    - 하나의 뷰 안에서 사용되는 값

```Swift
struct ContentView: View {
    @State private var didSelected = false
    
    var buttonImage: String {
        if didSelected {
            return "circle"
        } else {
            return "pencil"
        }
    }
    
    var body: some View {
        VStack {
            Text("current Status : \(didSelected.description)")
            
            Button(action: {
                didSelected.toggle()
            }, label: {
                HStack{
                    Image(systemName: buttonImage)
                        .resizable()
                        .aspectRatio(contentMode: .fit)
                    VStack {
                        Text("Button \(didSelected.description)")
                        Text("Button \(didSelected.description)")
                    }
                }
            })
            
        }
        .frame(width: 200, height: 120)
    }
}
```

### Toggle

```Swift
//
//  ContentView.swift
//  Test
//
//  Created by 나성희 on 2021/08/30.
//

import SwiftUI

// 바인딩 -> 원본과 연결되어 있다. 값을 대입할 수 없고 타입만 지정 가능
// 다른뷰에서 값을 변경하지만 원래 소스에도 연결되어 변경됨

struct ContentView: View {
    @State private var isOn = false
    
    var body: some View {
        VStack {
            MyToggle(isOn: $isOn)
            Text("\(isOn.description)")
        }
    }
}

struct MyToggle: View {
    
    @Binding var isOn: Bool
    
    var body: some View {
        Toggle("toggle \(isOn.description)", isOn: $isOn)
    }
}


struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
 
```