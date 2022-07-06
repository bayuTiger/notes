# サーブレット

## リクエストの処理

- HTTPリクエストに対応したメソッドが用意されており、メソッド名以外のメソッド定義は共通している

```java
protected void doGet(
  HttpServletRequest request
  HttpServletResponse response
)
throws ServletException, IOException{
  // 処理
}
```

- リクエスト情報は`get〇〇`の形で受け取ることができる
  - 例えば`getParameter`ではクエリ文字列の値を受け取ることができる
  - `?name=hoge&age=10`であれば、`request.getParameter("name")`をすると、hogeを受け取ることができる

## レスポンスの処理

- 処理を指示するための主な方法として**Forward**と**Redirect**の2種類がある
- Forward
  - 主にServletからServletに処理を渡す機能を持つ**処理の橋渡し役**
- Redirect
  - 他のサーバなどにリクエストを自動的に転送する**処理の移譲役**

## JSPから処理を受け取る

- JSP側

  ```java
  ~~
  <form action "処理を転送したいServletファイルへのパス" method="post">
  // inputやらの記述
  </form>
  ~~
  ```

- Servlet側:処理を受け取り、次の画面に転送する

  ```java
  // A画面から値を受け取って、B画面に受け取った値を表示するためのServletファイル

  // 文字化けする場合には必要
  request.setCharacterEncoding("UTF-8");
  String name = request.getParameter("取得したいもの");
  // 処理の受け渡しにはsetAttributeを使う。getParameterはあくまで受け取るだけ
  request.setParameter("取得したもののキー", 取得したものの値);
  // 次の画面を準備する。getResponseではなくgetRequestなのは、処理の転送中だから
  RequestDispatcher r = request.getRequestDispatcher(転送元のJSPファイル名);
  // 次の画面に転送する
  r.forward(request, response);
  ```
  
  ```java
  // B画面(JSP)

  ~~~
  <p>こんにちは、<%=request.getAttribute("name")%>さん</p>
  ~~~
  ```

## DBとの接続

- JDBCドライバを利用して接続する
- 基本的に、JDBCの決められた手順に沿って記述していくことになる
  - 行いたい操作によって、各手順の記述を変えていく
- DB処理に使用する特殊なオブジェクトは以下の3つ
  - Connection : データベースに接続する
  - PreparedStatement : SQL文を解析する
    - SQL文を事前に解析する機能を持ち、解析後も「?(プレースホルダー)」を任意の値に置き換えることが可能
  - ResultSet : SELECT文の結果を保持する
    - SELECTした結果そのものをオブジェクトとして保持している
    - 処理は各行に行い、`rs.next`メソッドを一回実行する毎に次の行の処理を行う
- 上記3つの処理はDBとの接続状態を保持しているため、繋ぎっぱなしにしていると接続数の枯渇を招いてしまう
  - したがって処理が終わる際にはclose処理を行う

- 以下はINSERT文の例

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class MySQLConnection {
	public static void main(String[] args) {
		
		Connection con = null;
		PreparedStatement ps = null;
		
		try {
			// JDBCドライバの登録
			Class.forName("com.mysql.cj.jdbc.Driver");
			
			// データベースへの登録
			String url = "jdbc:mysql://localhost/user";
			String user = "root";
			String password = "";
			con = DriverManager.getConnection(url, user, password);
			
			// ステートメントオブジェクトの取得
			String sql = "INSERT INTO user_list(name, age) VALUE('山田',20)";
			ps = con.prepareStatement(sql);
			
			// SQL操作
			int count = ps.executeUpdate();
			System.out.println(count + "件処理しました");
		
		}catch(ClassNotFoundException e) {
			e.printStackTrace();
		}catch(SQLException e) {
			e.printStackTrace();
		}finally {
			// DBとの接続終了
			try {
				if(con != null) {
					con.close();
				}
				if(ps != null) {
					ps.close();
				}
			}catch(Exception e) {
				e.printStackTrace();
			}
		}
	}
}
```

## スコープオブジェクト

- データの使用範囲と期限によって3種類に分けられる
  - アプリケーション
    - そのアプリケーション内で使用できる範囲
    - アプリケーション全体で情報が共有できる
    - アプリケーション起動中は有効
    - Servlet
      - `getServletContext`メソッドを実行する
    - JSP
      - 暗黙オブジェクトapplicationを使用する
  - リクエスト
    - リクエストを処理する際に使用できる範囲
    - 主に一時的に使用するデータを格納する
    - レスポンスされるまで有効
    - Servlet
      - doPostやdoGetメソッドの第一引数に自動的にセットされている
    - JSP
      - 暗黙オブジェクトrequestを使用する
  - セッション
    - リクエストしたクライアント単位で使用できる範囲
    - クライアント毎に個別のデータを格納する
    - セッションが破棄されるまで有効
    - Servlet
      - `getSession`メソッドを実行する
    - JSP
      - 暗黙オブジェクトsessionを使用する
