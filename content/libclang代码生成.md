---
title: libclang代码生成
date: 2024-12-19T10:16:27+08:00
categories: [C++]
tags: [C++,Utils]
---

## 安装clang工具链

``` bash
sudo apt-get install clang-10 lldb-10 lld-10
```

## python安装相关包
``` bash 
pip install clang==10.0.1
```

## clang++ 命令生成抽象语法树
```bash 
#打印AST
clang++ -Xclang -ast-dump -fsyntax-only 目标文件.h

#将AST保存到二进制文件(目标文件.h.gch)
clang++ -emit-ast 目标文件.h
```

## 例：


```cpp
//待解析源文件
class Box
{
   public:
      double length;   // 长度
      double breadth;  // 宽度
      double height;   // 高度
      // 成员函数声明
      double get(void);
      void set( double len, double bre, double hei );
};
// 成员函数定义
double Box::get(void)
{
    return length * breadth * height;
}
 
void Box::set( double len, double bre, double hei)
{
    length = len;
    breadth = bre;
    height = hei;
}
```
```Python
#生成代码如下
TranslationUnitDecl 0x3a8f2048 <<invalid sloc>> <invalid sloc>
|-TypedefDecl 0x3a8f2920 <<invalid sloc>> <invalid sloc> implicit __int128_t '__int128'
| `-BuiltinType 0x3a8f25e0 '__int128'
|-TypedefDecl 0x3a8f2990 <<invalid sloc>> <invalid sloc> implicit __uint128_t 'unsigned __int128'
| `-BuiltinType 0x3a8f2600 'unsigned __int128'
|-TypedefDecl 0x3a8f2d08 <<invalid sloc>> <invalid sloc> implicit __NSConstantString '__NSConstantString_tag'
| `-RecordType 0x3a8f2a80 '__NSConstantString_tag'
|   `-CXXRecord 0x3a8f29e8 '__NSConstantString_tag'
|-TypedefDecl 0x3a8f2da0 <<invalid sloc>> <invalid sloc> implicit __builtin_ms_va_list 'char *'
| `-PointerType 0x3a8f2d60 'char *'
|   `-BuiltinType 0x3a8f20e0 'char'
|-TypedefDecl 0x3a930018 <<invalid sloc>> <invalid sloc> implicit __builtin_va_list '__va_list_tag [1]'
| `-ConstantArrayType 0x3a92ffc0 '__va_list_tag [1]' 1 
|   `-RecordType 0x3a8f2e90 '__va_list_tag'
|     `-CXXRecord 0x3a8f2df8 '__va_list_tag'
|-CXXRecordDecl 0x3a930070 <../codegen/box.h:1:1, line:10:1> line:1:7 class Box definition
| |-DefinitionData pass_in_registers aggregate standard_layout trivially_copyable pod trivial literal
| | |-DefaultConstructor exists trivial needs_implicit
| | |-CopyConstructor simple trivial has_const_param needs_implicit implicit_has_const_param
| | |-MoveConstructor exists simple trivial needs_implicit
| | |-CopyAssignment trivial has_const_param needs_implicit implicit_has_const_param
| | |-MoveAssignment exists simple trivial needs_implicit
| | `-Destructor simple irrelevant trivial needs_implicit
| |-CXXRecordDecl 0x3a930188 <col:1, col:7> col:7 implicit class Box
| |-AccessSpecDecl 0x3a930218 <line:3:4, col:10> col:4 public
| |-FieldDecl 0x3a930258 <line:4:7, col:14> col:14 referenced length 'double'
| |-FieldDecl 0x3a9302c0 <line:5:7, col:14> col:14 referenced breadth 'double'
| |-FieldDecl 0x3a930328 <line:6:7, col:14> col:14 referenced height 'double'
| |-CXXMethodDecl 0x3a930470 <line:8:7, col:22> col:14 get 'double ()'
| `-CXXMethodDecl 0x3a930708 <line:9:7, col:52> col:12 set 'void (double, double, double)'
|   |-ParmVarDecl 0x3a930528 <col:17, col:24> col:24 len 'double'
|   |-ParmVarDecl 0x3a9305a8 <col:29, col:36> col:36 bre 'double'
|   `-ParmVarDecl 0x3a930628 <col:41, col:48> col:48 hei 'double'
|-CXXMethodDecl 0x3a930898 parent 0x3a930070 prev 0x3a930470 <line:12:1, line:15:1> line:12:13 get 'double ()'
| `-CompoundStmt 0x3a930ae0 <line:13:1, line:15:1>
|   `-ReturnStmt 0x3a930ad0 <line:14:5, col:31>
|     `-BinaryOperator 0x3a930ab0 <col:12, col:31> 'double' '*'
|       |-BinaryOperator 0x3a930a38 <col:12, col:21> 'double' '*'
|       | |-ImplicitCastExpr 0x3a930a08 <col:12> 'double' <LValueToRValue>
|       | | `-MemberExpr 0x3a930998 <col:12> 'double' lvalue ->length 0x3a930258
|       | |   `-CXXThisExpr 0x3a930988 <col:12> 'Box *' implicit this
|       | `-ImplicitCastExpr 0x3a930a20 <col:21> 'double' <LValueToRValue>
|       |   `-MemberExpr 0x3a9309d8 <col:21> 'double' lvalue ->breadth 0x3a9302c0
|       |     `-CXXThisExpr 0x3a9309c8 <col:21> 'Box *' implicit this
|       `-ImplicitCastExpr 0x3a930a98 <col:31> 'double' <LValueToRValue>
|         `-MemberExpr 0x3a930a68 <col:31> 'double' lvalue ->height 0x3a930328
|           `-CXXThisExpr 0x3a930a58 <col:31> 'Box *' implicit this
`-CXXMethodDecl 0x3a930cb8 parent 0x3a930070 prev 0x3a930708 <line:17:1, line:22:1> line:17:11 set 'void (double, double, double)'
  |-ParmVarDecl 0x3a930b18 <col:16, col:23> col:23 used len 'double'
  |-ParmVarDecl 0x3a930b98 <col:28, col:35> col:35 used bre 'double'
  |-ParmVarDecl 0x3a930c18 <col:40, col:47> col:47 used hei 'double'
  `-CompoundStmt 0x3a95e1c8 <line:18:1, line:22:1>
    |-BinaryOperator 0x3a930e38 <line:19:5, col:14> 'double' lvalue '='
    | |-MemberExpr 0x3a930dd0 <col:5> 'double' lvalue ->length 0x3a930258
    | | `-CXXThisExpr 0x3a930dc0 <col:5> 'Box *' implicit this
    | `-ImplicitCastExpr 0x3a930e20 <col:14> 'double' <LValueToRValue>
    |   `-DeclRefExpr 0x3a930e00 <col:14> 'double' lvalue ParmVar 0x3a930b18 'len' 'double'
    |-BinaryOperator 0x3a930ed0 <line:20:5, col:15> 'double' lvalue '='
    | |-MemberExpr 0x3a930e68 <col:5> 'double' lvalue ->breadth 0x3a9302c0
    | | `-CXXThisExpr 0x3a930e58 <col:5> 'Box *' implicit this
    | `-ImplicitCastExpr 0x3a930eb8 <col:15> 'double' <LValueToRValue>
    |   `-DeclRefExpr 0x3a930e98 <col:15> 'double' lvalue ParmVar 0x3a930b98 'bre' 'double'
    `-BinaryOperator 0x3a95e1a8 <line:21:5, col:14> 'double' lvalue '='
      |-MemberExpr 0x3a95e140 <col:5> 'double' lvalue ->height 0x3a930328
      | `-CXXThisExpr 0x3a930ef0 <col:5> 'Box *' implicit this
      `-ImplicitCastExpr 0x3a95e190 <col:14> 'double' <LValueToRValue>
        `-DeclRefExpr 0x3a95e170 <col:14> 'double' lvalue ParmVar 0x3a930c18 'hei' 'double'
```