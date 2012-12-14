# spring トランザクション管理
spring3でアノテーションを使ったトランザクション管理についてざっくりまとめたので共有します。
まず、springトランザクションを使うメリットを簡単に。

# メリット
springトランザクションを使うことでコードが非常にすっきりします。特にアノテーションを使った方法がお勧め。コードを見るだけでトランザクション管理されているか分かります。
springトランザクションはaopを使用します。

# コードでトランザクション管理すると？
自分たちでトランザクション管理するコードを書いた場合、dao層にコネクションを渡す必要があったり、トランザクション開始終了やcommit,rollbackといったソースが散在するため
非常にコードが汚くなる。

# トランザクションの境界
コントローラからサービスクラスが呼び出されたらトランザクション開始。 サービスクラスが終了してコントローラに戻るときがトランザクション終了。

# トランザクションマネージャで設定できる値
* 伝搬属性：デフォルトはPROPAGATION_REQUIRED
* 独立性レベル：デフォルトはISOLATION_READ_COMMITTED
* タイムアウト値：デフォルトは-1(タイムアウトしない)
* 読み取り専用有無：デフォルトはfalse
* ロールバック対象例外：デフォルトは実行時例外がロールバック対象
* コミット対象例外：デフォルトは検査例外がコミット対象

`TransactionDefinition`参照

# 使用するとトランザクションマネージャーの実装

# トランザクションの記述方法
アノテーションかxml記述が良いとされている。ソースコードべた書きは基本NG。springではアノテーションをお勧めしている。

## なぜアノテーションか？
アノテーションで記述するとソースコードを見るだけでトランザクション対象か否かが分かるし、トランザクションマネージャーの設定内容も分かり、トレースしやすい。

---

# アノテーション記述方法
    @Transactional
    public class ServiceImpl {
    	public void insert(Hoge hoge) {
    		//	insert処理
    	}
    	
    	public void update(Hoge hoge) {
    		//	update処理
    	}
    	
    	public void search(String id) {
    		//	update処理
    	}
    }

上記で ServiceImplのすべてのメソッドがトランザクション処理される。またメソッドでトランザクション情報を変更したい場合はメソッドで@Transaction情報を上書きする.

    @Transactional
    public class ServiceImpl {
    	@Transactional(timeout=120)
    	public void insert(Hoge hoge) {
    		//	insert処理
    	}
    	
    	public void update(Hoge hoge) {
    		//	update処理
    	}
    	
    	@Transactional(reandOnly=true)
    	public void search(String id) {
    		//	update処理
    	}
    }

## @Transactional(readOnly=true)
検索メソッドには@transactional(readOnly=true)を設定したほうがRMなどはread-onlyを最適化して処理するためパフォーマンスが出る。

# xmlの設定
	<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource" />
	</bean>
	
	<tx:annotation-driven />
	