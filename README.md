# PHP_FrontController

>フロントコントローラの役割

->大まかにいうとWebのプログラミング処理のアクセスポイントみたいなもの。</br>

->パス名とそのパス名に対応する処理をマッピングしたルール処理を「ルーティング情報」という。</br>

  ・ルーティング情報とは、パス名とプログラム処理をマッピングのみしたものであり、</br>
  　その情報をもとに、ディスパッチャーがルールに従った処理を行う。</br>
   
   </br></br>
   
->フロントコントローラからアクセスされたパス名をディスパッチされる</br>
->一般的には、アクセスするURLから、パス(フロントコントローラ等)を隠すために、Apacheの[.htaccess]ファイル内に記述する。</br>
　　　　Apacheの設定方法(.htaccess編)</br>
          ・ドキュメント直下にある。</br>
           {</br>
                RewriteEngine On</br>
                RewriteCond %{REQUEST_FILENAME}!-f #ファイルの実体があるか</br>
                RewriteCond %{REQUEST_FILENAME}!-d #ドキュメントの実体があるか</br>
                RewriteRule ^(.*)$index.php[L]</br>
           }</br>
    
>フロントコントローラの処理イメージ</br>

1.Webクライアントが、以下のURIにアクセスする。</br>
　結果として、[index.php]のプログラム処理が実行される。
        http://example/com/user
        
2.この際の$_SERVER['REQUEST_URL']の値は、[/user]となる。
            ↑この__SERVERはphpのスーパーグローバル変数

3.index.phpが、この値をディスパッチャーとして渡すことで、コントローラクラスが起動する。


>※PHPの文法(復習用)※
  ・クロージャ：関数の中で定義された変数、及び関数の結果がセットで保存されている構文方式
  
  ・declare(strict_types=1); #型を制限するための宣言、コードの冒頭に記述。
  
  ・require , require_once #ユーザ定義関数は、複数のスクリプトファイルで共有することが多い。
                           #そのため、再利用性を高めるためにユーザ定義関数を別ファイルとして保存しておき、
                           #必要に応じて[require],[require_once]する。

  ・オートローダー(概要)：複数のスクリプトを再利用するという性質上、「1クラスを1ファイル」で管理するべきである。
  　　　　　　　　　      ファイル管理という観点から、整理しやすくなるメリットがある。
           　　　　      しかし、扱うクラスが増えてきた際に、クラスファイルを一つずつ、[require_once]で呼び出さなくてはいけなくなり
               　　      記述漏れの原因となりえる。
                       　そこで利用されるのが[spl_autoload_register]関数
                        
  ・オートローダー(ライブラリ「composer」を使用しない場合)
  
#spl_autoload_register関数を自作し、実装する。
  
Autoloader.php
  <?php

  spl_autoload_register(function($name){
                  require_once "{$name}.php";
  });

-----

Person.php
  <?php
  $p = new Person('太郎','山田'); 

#1.未定義のクラス呼び出し
#2.オートローダの自動生成
#3.Person.phpのインクルード処理
#4.Personクラスのインスタンス化


