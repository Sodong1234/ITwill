# [오전수업] DB 31차
## 제약조건 일시적으로 해제하기
- 제약조건의 상태가 enabled인 경우 제약조건이 정상적으로 동작하는 상태
```sql

SELECT constraint_name,
       constraint_type,
       status
FROM user_constraints
WHERE table_name = 'EMP2';


CONSTRAINT_NAME|CONSTRAINT_TYPE|STATUS |
---------------+---------------+-------+
SYS_C007714    |C              |ENABLED|
SYS_C007715    |C              |ENABLED|
SYS_C007716    |C              |ENABLED|
SYS_C007717    |C              |ENABLED|
EMP2_EID_PK    |P              |ENABLED|
```

- 제약조건을 일시적 해제가 필요한 경우 DISABLE 시키는 것도 가능하다.
```sql
ALTER TABLE emp2
DISABLE CONSTRAINT EMP2_EID_PK;


SELECT constraint_name,
       constraint_type,
       status
FROM user_constraints
WHERE table_name = 'EMP2';


CONSTRAINT_NAME|CONSTRAINT_TYPE|STATUS  |
---------------+---------------+--------+
SYS_C007714    |C              |ENABLED |
SYS_C007715    |C              |ENABLED |
SYS_C007716    |C              |ENABLED |
SYS_C007717    |C              |ENABLED |
EMP2_EID_PK    |P              |DISABLED|
```

- 206번 사원의 사번을 100으로 갱신
```sql

UPDATE emp2
SET employee_id = 100
WHERE employee_id = 206;


SELECT employee_id, last_name
FROM emp2
WHERE employee_id = 100;

EMPLOYEE_ID|LAST_NAME|
-----------+---------+
        100|King     |
        100|Gietz    |
```

- 기존 employee_id 컬럼은 PK제약조건이 적용되어 NULL값과 중복값을 허용하지 않으나 일시적으로 제약조건을 끄는 경우 설정 된 제약조건의 영향을 받지 않게 된다.
```sql
ALTER TABLE emp2
ENABLE CONSTRAINT EMP2_EID_PK;

```

- 제약조건 재활성 시 입력 값이 제약조건을 만족하지 않는 경우 아래와 같이 제약조건을 활성할 수 없다.
* SQL Error [2437] [72000]: ORA-02437: (HR.EMP2_EID_PK)을 검증할 수 없습니다 - 잘못된 기본 키입니다

- 다시 제약조건에 맞는 값으로 수정한 뒤에는 제약조건을 재활성에 문제없이 동작함.
```sql

UPDATE emp2
SET employee_id = 206
WHERE last_name = 'Gietz';


SELECT constraint_name,
       constraint_type,
       status
FROM user_constraints
WHERE table_name = 'EMP2';

CONSTRAINT_NAME|CONSTRAINT_TYPE|STATUS |
---------------+---------------+-------+
SYS_C007714    |C              |ENABLED|
SYS_C007715    |C              |ENABLED|
SYS_C007716    |C              |ENABLED|
SYS_C007717    |C              |ENABLED|
EMP2_EID_PK    |P              |ENABLED|
```

## DROP TABLE
- 기존 테이블을 삭제하는 문법
- 삭제하려는 테이블이 부모테이블인 경우 제약조건 삭제전에는 테이블을 삭제할 수 없다.
```sql
DROP TABLE emp2;
```

- 외래키 제약조건으로 엮어져 있는 자식테이블의 데이터를 포함하여 삭제하는 경우 CASCADE CONSTRAINTS 옵션을 사용하면 된다.
```sql
DROP TABLE 테이블명 [CASCADE CONSTRAINTS];
```

## FLASHBACK TABLE 기술
- ORACLEDB에서는 테이블을 삭제하는 경우 해당 테이블을 삭제할 수 없게된다.
- 다만, 테이블을 실수로 삭제했을 경우에 대비하여 바로 물리적으로 삭제하지 않고, 사용 할 수 없게 가려두는 방식으로 테이블을 일시적으로 보관해둔다.

```sql
DROP TABLE emp2;


SELECT original_name, operation, droptime
FROM recyclebin;

ORIGINAL_NAME|OPERATION|DROPTIME           |
-------------+---------+-------------------+
EMP2_EID_PK  |DROP     |2022-05-18:10:20:30|
EMP2         |DROP     |2022-05-18:10:20:30|
```

- recyclebin에서 조회가 되고 있는 경우 drop을 취소하고 다시 테이블을 원상복구 시킬 수 있다.
```sql

FLASHBACK TABLE emp2 TO BEFORE DROP;


SELECT * FROM tab;

TNAME           |TABTYPE|CLUSTERID|
----------------+-------+---------+
COUNTRIES       |TABLE  |         |
DEPARTMENTS     |TABLE  |         |
DEPT80          |TABLE  |         |
EMP2            |TABLE  |         |
EMPLOYEES       |TABLE  |         |
EMP_DETAILS_VIEW|VIEW   |         |
JOBS            |TABLE  |         |
JOB_HISTORY     |TABLE  |         |
LOCATIONS       |TABLE  |         |
REGIONS         |TABLE  |         |
```

## PURGE옵션
- 테이블삭제 시 PURGE옵션을 추가하여 삭제하는 경우 recyclebin을 거치지 않고 바로 물리적으로 테이블을 삭제하게 된다.
```sql

DROP TABLE emp2 PURGE;


SELECT original_name, operation, droptime
FROM recyclebin;

ORIGINAL_NAME|OPERATION|DROPTIME|
-------------+---------+--------+
```

## TRUNCATE 문법
- 테이블의 데이터 저장공간을 잘라서 자원을 데이터베이스 돌려주는 문법


![unnamed](https://user-images.githubusercontent.com/95197594/168948238-eee6c736-e66a-4a07-8a91-aad83a6cf017.png)

- DELETE 문법은 저장공간은 유지하기 때문에 다시 데이터 입력이 가능하다
- TRUNCATE문법은 저장공간을 잘라서 데이터베이스 반환하는 문법으로 재사용 시 공간의 재할당이 필요.

```sql
TRUNCATE TABLE dept80;


SELECT * FROM dept80;

EMPLOYEE_ID|LAST_NAME|ANNSAL|HIRE_DATE|
-----------+---------+------+---------+
```

## INDEX
- 테이블의 데이터를 조회 시 일부 검색 성능을 올려주는 오브젝트
- 인덱스 생성 시 해시함수의 결과들로 인덱스를 생성하여 빠른 검색이 가능하도록 해준다.


![unnamed (1)](https://user-images.githubusercontent.com/95197594/168953036-1ac8c384-c8f6-43ea-9569-8b277165e330.png)

- 인덱스가 최적으로 효과를 발휘하려면,
  - 데이터가 많을것
  - 값의 갱신이 적을 것
  - 검색조건으로 사용될 것

- PK, UK 제약조건이 설정된 컬럼인 경우 자동으로 INDEX가 생성된다.


## SYNONYM
- 기존 오브젝트에 다른 이름을 부여하는 오브젝트
- 간편한 이름을 오브젝트에 부여하는 경우
- 사용하는 오브젝트를 바꾸고 싶을 때


![unnamed (2)](https://user-images.githubusercontent.com/95197594/168953142-fa220dc3-4939-4e49-a997-e5917f45af65.png)
- employees테이블을 가리키는 emp 시노님 생성
```sql

CREATE SYNONYM emp
FOR employees;


SELECT * FROM emp;
```

---

# [오후수업] JSP 67차

## 복호화

```java
-----------------------------------------ExRSAKeyGenerator.java-----------------------------------------
package cipher;

import java.security.KeyPair;
import java.security.KeyPairGenerator;
import java.security.NoSuchAlgorithmException;
import java.security.PrivateKey;
import java.security.PublicKey;
import java.security.SecureRandom;

public class ExRSAKeyGenerator {

	public static void main(String[] args) {
		// RSAKeyGenerator 객체를 통해 공개키와 개인키를 생성한 후 리턴받아 출력
		RSAKeyGenerator keyGenerator = new RSAKeyGenerator();
		PublicKey publicKey = keyGenerator.getPublicKey();
		PrivateKey privateKey = keyGenerator.getPrivateKey();

		System.out.println(publicKey);
		System.out.println(privateKey);


	}

}

class RSAKeyGenerator {
	private KeyPair keyPair;
	
	public RSAKeyGenerator() {
		// RSA 알고리즘을 사용하여 공개키와 개인키 한 쌍(KeyPair) 생성
		// KeyPairGenerator 클래스의 getInstance() 메서드를 호출하여 객체 얻어오기
		// => 파라미터로 암호화 알고리즘명 전달("RSA" 전달)
		try {
			KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance("RSA");
			
			// 암호키의 길이를 설정하기 위한 작업
			SecureRandom secureRandom = new SecureRandom();
			// KeyPairGenerator 객체의 initialize() 메서드를 호출하여 코드길이와 난수 객체(SecureRandom) 전달
//			keyPairGenerator.initialize(4096, secureRandom); // 4096 bit 코드 설정(기본값 2048 bit)
			
			// KeyPairGenerator 객체의 generateKeyPair() 또는 genKeyPair() 메서드를 호출하여 암호키(공개, 개인) 생성
			keyPair = keyPairGenerator.generateKeyPair(); // 공개키와 개인키 생성됨
		} catch (NoSuchAlgorithmException e) {
			e.printStackTrace();
			System.out.println("입력한 암호화 알고리즘이 존재하지 않습니다!");
		}

	}
	
	// 공개키와 개인키를 각각 리턴하는 Getter 정의
	// => 공개키는 PublicKey 타입 리턴, 개인키는 PrivateKey 타입 리턴
	public PublicKey getPublicKey() {
		// 이미 생성되어 있는 KeyPair 객체의 getPublic() 메서드를 호출하여 공개키 리턴받기
//		PublicKey publickey = keyPair.getPublic();
		return keyPair.getPublic();
	}
	
	public PrivateKey getPrivateKey() {
		return keyPair.getPrivate();
	}
	
	
	
}

-----------------------------------------ExRSACipher.java-----------------------------------------

package cipher;

import java.math.BigInteger;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.security.PrivateKey;
import java.security.PublicKey;
import java.util.Base64;

import javax.crypto.BadPaddingException;
import javax.crypto.Cipher;
import javax.crypto.IllegalBlockSizeException;
import javax.crypto.NoSuchPaddingException;

public class ExRSACipher {

	public static void main(String[] args) {
		/*
		 * RSA 알고리즘
		 * - 공개키 방식의 암호화 알고리즘(공개키와 개인키를 사용하여 암호화 및 복호화 수행)
		 * - 인수분해를 기반으로 암호 생성하는 방식
		 * - Rivest, Shamir, Adleman 이라는 사람의 성을 한 글자씩 따서 RSA 라고 지음
		 * --------------------------------------------------------------------
		 * 
		 */
		// RSAKeyGenerator 객체를 통해 공개키와 개인키를 생성한 후 리턴받아 출력
		RSAKeyGenerator keyGenerator = new RSAKeyGenerator();
		PublicKey publicKey = keyGenerator.getPublicKey();
		PrivateKey privateKey = keyGenerator.getPrivateKey();

		System.out.println(publicKey);
		System.out.println(privateKey);
		
		System.out.println("================================================");
		

		try {
			RSACipher rsaCipher = new RSACipher("admin123", publicKey, privateKey);
			String encryptedText = rsaCipher.encrypt();
			System.out.println("암호화 결과 : " + encryptedText);
//			System.out.println("암호화 된 문자열 길이 : " + encryptedText.length());
			
			
			System.out.println("=================================================");
			
//			String decryptedText = rsaCipher.decrypte(encryptedText);
			
			String decryptedText = rsaCipher.decrypt(encryptedText);
			System.out.println("복호화 결과 : " + decryptedText);
			
			
		} catch (InvalidKeyException | NoSuchAlgorithmException | NoSuchPaddingException | IllegalBlockSizeException
				| BadPaddingException e) {
			e.printStackTrace();
		}
	}

}

// 암호화를 수행할 RSACipher 클래스 정의
class RSACipher {
	Cipher cipher; // 암호화 및 복호화 작업을 수행할 javax.crypto.Cipher 타입 변수 선언
	
	// 정보를 저장할 변수 선언
	private String strPlainText; // 평문을 저장할 멤버변수
	private PublicKey publicKey; // 공개키 저장용
	private PrivateKey privateKey; // 개인키 저장용
	
	// 파라미터 생성자를 정의하여 평문, 공개키, 개인키를 전달받아 초기화
	public RSACipher(String strPlainText, PublicKey publicKey, PrivateKey privateKey) throws NoSuchAlgorithmException, NoSuchPaddingException {
		this.strPlainText = strPlainText;
		this.publicKey = publicKey;
		this.privateKey = privateKey;
		
		// Cipher 클래스의 getInstance() 메서드를 호출하여 Cipher 객체 얻어오기(암호화 알고리즘 선택)
		this.cipher = Cipher.getInstance("RSA");
		
	}
	
	// 암호화 작업을 수행하는 encrypt() 메서드 정의
	// => 파라미터 : 없음	 리턴타입 : String
	public String encrypt() throws InvalidKeyException, IllegalBlockSizeException, BadPaddingException {
		// Cipher 객체의 init() 메서드를 호출하여 암호화 작업 준비
		// => 파라미터로 ENCRYPT_MODE 상수와 공개키 전달
		cipher.init(Cipher.ENCRYPT_MODE, publicKey);
		
		// Cipher 객체의 doFinal() 메서들르 호출하여 암호화 작업 수행
		// => 파라미터 : 평문에 대한 byte[] 타입 
		byte[] encryptedBytes = cipher.doFinal(strPlainText.getBytes());
		
		String encryptedText = null;
		// 암호화 된 byte[] 타입 데이터를 BigInteger 객체를 통해 정수 데이터로 변환한 후 다시 문자열로 변환하여 리턴
		BigInteger bi = new BigInteger(encryptedBytes);
		encryptedText = bi.toString();
		
//		System.out.println("암호화 결과 : " + encryptedText);
//		System.out.println("암호화 된 문자열 길이 : " + encryptedText.length());
		
		return encryptedText; // 암호화 문자열 리턴
	}
	
//	 복호화 작업을 수행하는 decrypt() 메서드 정의
//	 => 파라미터 : 암호화 문자열, 리턴타입 : String
	public String decrypt(String encryptedText) throws InvalidKeyException, IllegalBlockSizeException, BadPaddingException {
		// 암호화 과정과 초기화 작업을 제외한 나머지 과정은 동일
		// 초기화 과정에서 ENCRYPT_MODE 대신 DECRYPT_MODE, 공개키 대신 개인키 전달
		cipher.init(Cipher.DECRYPT_MODE, privateKey); // 개인키를 사용하여 복호화 작업 준비
		
		byte[] decryptedBytes = cipher.doFinal(new BigInteger(encryptedText).toByteArray()); // 복호화
		
		// 복호화 된 데이터를 문자열로 변환
		String decryptedText = null;
		decryptedText = new String(decryptedBytes);
		
		return decryptedText;
		
	}
	
}

```
