# ELEC1005_G9

Page_1  Welcome:
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


Page_3 AccountLogin1
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
