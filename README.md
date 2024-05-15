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
  Button3: 
  If(
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
    
  Search_botton: 
    If(
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
    Cart2: false

Page_7 Cart1
  Label7_3: "Cart"
  Label7_2: "Search"
  Icon2_2: Navigate(Search1,ScreenTransition.CoverRight)
  Icon1_1: Navigate(Cart1,ScreenTransition.Cover)
  Rectangle18_4: false
  Cart: "Cart"
  Line2: false
  Gallery:
    Rectangle1: Select(Parent)
    Separator: Select(Parent)
    Subtitle1: LookUp('good-info', 'good-name' = ThisItem.Value, 'good-location')
    Title1: ThisItem.Value
    Image1: LookUp('good-info', 'good-name' = ThisItem.Value, 'good-image')
    NextArrow1: 
    If(
      !IsBlank(
          LookUp(
              'good-info',
              'good-name' = ThisItem.Value
        )
    ),
    Navigate(Good1, ScreenTransition.Cover),
    Navigate(GoodNotFound, ScreenTransition.Cover)
    );
    If(
      !IsBlank(
          LookUp(
              'good-info',
              'good-name' = ThisItem.Value
          )
      ),
      Set(globalGoodName, ThisItem.Value)
    );


Page_8 Good1
  Label6: "Add to cart"
  Icon5: false
  Label3_1: LookUp('good-info', 'good-name' = globalGoodName, 'good-price')
  Label3: LookUp('good-info', 'good-name' = globalGoodName, 'good-name')
  Icon4: false
  Icon2_8: false
  Back_15: "Back"
  Image1: LookUp('good-info', 'good-name' = globalGoodName, 'good-image')
  Manufacter: LookUp('good-info', 'good-name' = globalGoodName, 'good-details')
  Ellipse4: false
  AvailableType: LookUp('good-info', 'good-name' = globalGoodName, 'good-available')
  Detail_2: "Details"
  Location_2: "Location"
  01.39.0831: LookUp('good-info', 'good-name' = globalGoodName, 'good-location')
  Rectangle16: false
  Rectangle4_1: 
    //Navigate(Search1, ScreenTransition.Fade)
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
    Back(ScreenTransition.Fade)
  Button2: 
    // 从 'good-info' 数据集中查找商品名称并存储在全局变量中
    Set(currentGoodName, LookUp('good-info', 'good-name' = globalGoodName, 'good-name'));
    // 查找用户记录
    // 从 'good-info' 数据集中查找商品名称并存储在全局变量中
    Set(currentGoodName, LookUp('good-info', 'good-name' = globalGoodName, 'good-name'));
    // 查找用户记录
    Set(currentUser, LookUp('user-info', 'Email-Address' = globalUserEmail));
    // 检查并从购物车中删除商品
    If(
        Not(IsBlank(currentUser)),
        // 判断当前购物车中是否存在该商品
        If(
            !IsBlank(
                Find(currentGoodName, Concat(Split(currentUser.'cart-list', Char(10)), Value & Char(10)))
            ),
            // 通过 Split 拆分购物车列表，过滤掉需要删除的商品，并排除空行
            Patch(
                'user-info',
                currentUser,
                {'cart-list': Concat(
                    Filter(
                        Split(currentUser.'cart-list', Char(10)),
                        Value <> currentGoodName && !IsBlank(Value)
                    ),
                    Value & Char(10)
                )}
            );
            Notify("The item was successfully deleted.", NotificationType.Success)
        ),
        Notify("商品不存在于购物车", NotificationType.Warning)
    )


Page_9 GoodNotFound
  Image2: 'AA9DB1E0-FF8B-4E1B-A368-52A4C7AB254C'
  Icon2_4: Back(ScreenTransition.Fade)
  Back_12: "Back"
  Sorry, We Not Found: "Sorry,
    We Not Found."


Page_10 ForgetPassword
  Image7: 'AC69F863-71AB-4198-AFEE-52CF6A43FBBC'
  Rectangle4_3: Back(ScreenTransition.Fade)
  Icon2_10: false
  Back_17: "Back"


Page_11 UserPolicy
  Image6: 'D5FC7114-F0E5-4D31-A7F3-81940AA10424'
  Rectangle4_2: Back(ScreenTransition.Fade)
  Icon2_9: false
  Back_16: "Back"
