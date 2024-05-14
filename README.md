# ELEC1005_G9

Page_1  Welcome
  Label4: "New user? Register here."
  Icon6: false
  "‎أهلاً وسهلاً"
  "欢迎"
  "ようこそ"
  "Добро пожаловать"
  "Willkommen"
  "Bienvenido"
  "Bienvenue"
  "Welcome"
  "Log In"
  Button1: Navigate(AccountLogIn1, ScreenTransition.None)


Page_2 AccountRegister
  Icon4_3: false
  *Required_2: "Name:"
  *Required_3: "Email:"
  *Required_4: "Password:"
  NameInput: ""
  Icon2_1: Back(ScreenTransition.CoverRight)
  Back_9: "Back"
  PasswordInput: ""
  EmailInput: ""
  Ellipse3: image36111
  Button3_1: 
  // 检查所有输入是否已填写
  If(
      IsBlank(NameInput.Text) || IsBlank(PasswordInput.Text) || IsBlank(EmailInput.Text),
      // 如果任何输入为空，显示错误消息
      Notify("Please complete all information.", NotificationType.Error),
      // 如果所有输入都已填写，执行 Patch 函数
      Set(varResult,
          Patch(
              'user-info',
              Defaults('user-info'),
              {
                  'user-name': NameInput.Text,
                  passwords: PasswordInput.Text,
                  'Email-Address': EmailInput.Text
              }
          )
    ),
    // 检查 Patch 操作的结果
    If(
        !IsBlank(varResult),
        // 如果结果非空（即 Patch 操作成功），导航到登录页面
        Navigate(AccountLogIn1,ScreenTransition.Cover),
        // 如果结果为空（即 Patch 操作失败），显示错误消息
        Notify("注册失败，请检查输入或网络连接", NotificationType.Error)
    )
)


Page_3 AccountLogIn1
  Icon2_7: //Back(ScreenTransition.CoverRight)
Navigate(Welcome,ScreenTransition.CoverRight)
  Back_14: "Back"
  Icon4_1: false
  Button7: Navigate(Welcome, ScreenTransition.None)
  TextInput1_1: ""
  TextInput1: ""
  Forget Password ? Do Not Click here.: "Forget Password ? Do Not Click here."
  Ellipse3_4: image36113
  Button3: If(
    !IsBlank(
        LookUp(
            'user-info', 
            'Email-Address' = TextInput1.Text && passwords = TextInput1_1.Text
        )
    ),
    Navigate(Search1,ScreenTransition.Cover),
    Navigate(LogInFail,ScreenTransition.Cover)
);

// Set global variable after successful login check
If(
    !IsBlank(
        LookUp(
            'user-info', 
            'Email-Address' = TextInput1.Text && passwords = TextInput1_1.Text
        )
    ),
    Set(globalUserEmail, TextInput1.Text)
);


Page_4 LogInFail
  Image3: 'F28F6333-968B-4B2D-BBEA-C946F77D7519'
  Label1: "Hmmm… "
  Label2: "we can’t seem to find that combination in our records. Please retry."
  Button5: Navigate(AccountLogIn1, ScreenTransition.CoverRight)


Page_5 AccountView
  Rectangle4: Navigate(Search1, ScreenTransition.Fade)
  Icon2_6: false
  Icon4_2: false
  Back_13: "Back"
  Rectangle2: Navigate(Welcome, ScreenTransition.None)
  Log Out: "Log Out"
  Rectangle2_1: image48334
  Our User Policy: "Our User Policy"
  Name: "Name: "
  Kamisato Ayaka: LookUp('user-info', 'Email-Address' = globalUserEmail).'user-name'
  E-mail: "E-mail: "
  114514@test.com: globalUserEmail
  Ellipse3_3: image48297
  Ellipse4_4: false
  Ellipse4_5: false


Page_6 Search1
  Label7_1: "Cart"
  Label7: "Search"
  Search_box: ""
  Ellipse4_3: false
  Ellipse3_1: image36111
  Ellipse4_1：false
  Ellipse4_2: false
  Icon2: false
  Icon4_4: false
  Rectangle19_3: false
  Rectangle18_3: false
  Rectangle6: false
  Image3_1: image4524
  Image2_1: image4522
  Image1_1: image4515
  Image5_1: image4528
  Recomed To You_1: "Recomed To You"
  Catalogues_1: "Catalogues"
  Button8: Navigate(AccountView, ScreenTransition.Fade)
  Button6: Launch("https://embed.salefinder.com.au/coles/flybuys/#view=catalogue&saleId=54632&locationId=8245&page=1")
  •••_1: "•
•
•"

  Icon1: 
Set(currentUser, LookUp('user-info', 'Email-Address' = globalUserEmail));
// 将购物车中的商品列表转换为集合，并排除空行
ClearCollect(
    CartItems,
    Filter(
        Split(currentUser.'cart-list', Char(10)),
        !IsBlank(Value)
    )
);

// 存储在全局变量中
Set(globalUserCart, CartItems);

Navigate(Cart1,ScreenTransition.Cover)
  
  Search_botton: If(
    !IsBlank(
        LookUp(
            'good-info', 
            'good-name' = Search_box.Text
        )
    ),
    Navigate(Good1,ScreenTransition.CoverRight),
    Navigate(GoodNotFound,ScreenTransition.CoverRight)
);

If(
    !IsBlank(
        LookUp(
            'good-info', 
            'good-name' = Search_box.Text
        )
    ),
    Set(globalGoodName, Search_box.Text)
);


