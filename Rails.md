Rails
---

基本操作
---

* rails new [app]  
   Appのひな形作成  
   rails new [app] --database=mysql --skip-bundle  
   DBをmysqlにbundle installをスキップしてくれる
* rails generate scaffold page name:string title:string  
   controllerとmodelを作成してくれる  
* rails generate model page name:string title:string  
   modelの作成  
   db/migrate/日付_create_page.rbができる  
* rails generate controller Page  
   controllerの作成  
   rails generate controller page title  
   controllerとviewの作成  
   rails generate controller 'admin/page'  
   階層化された  
* rails console  
   コンソールの起動  
* rails db  
   DBへのアクセス  
* rails server  
   サーバへアクセスする  
