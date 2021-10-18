사용 용도는 2가지가 있습니다.



### 1. 지시문(Directive)



다른 네임스페이스에 정의된 타입을 Import 하거나, 네임스페이스에 대한 별칭을 만들때 사용한다.


```
using System.Text; //코드 상단에 네임스페이스 정의
using Project = PC.MyCompany.Project; // 별칭
```



### 2. 문장(Statement) *



개체의 범위를 정의할때 사용한다. 그 범위를 벗어나면 자동으로 Dispose 된다.   
.         

File이나 Font, DB Connection 관련 클래스들은 관리되지 않는 리소스에 액세스 합니다. 다 사용후 적절하게 Dispose해서 자원을

반납해야 합니다. 하지만 종종 Dispose를 하지 않아서 리소스가 낭비되거나 DB Connection 같은 것을 Open만하고 Close하지 않으면

문제가 발생합니다. 이때 일일이 Close하지 않고 Using을 이용하면 그 범위를 벗어나면 자동으로 Dispose 되서 관리가 쉬워집니다.

```
using (SqlConnection connection = new SqlConnection(connectionString))
{
    SqlCommand command = new SqlCommand(queryString, connection);
    command.Connection.Open();
    command.ExecuteNonQuery();
}
```


위처럼 Connection을 Using 구문으로 사용하면 {} 범위를 벗어나면 자동으로 Dispose가 됩니다.

using 문은 개체의 메서드를 호출하는 동안 예외가 발생하는 경우에도 Dispose가 호출되도록 합니다. 

try 블록 내에 개체를 배치한 다음 finally 블록에서 Dispose를 호출해도 동일한 결과를 얻을 수 있습니다.    
   

.      
실제로 이 방식은 컴파일러에서 using 문이 변환되는 방식입니다.

이전의 코드 예제는 컴파일 타임에 다음 코드로 확장됩니다. 
```
{
    SqlConnection connection = new SqlConnection(connectionString);
 
    try 
    {            
        SqlCommand command = new SqlCommand(queryString, connection);
        command.Connection.Open();
        command.ExecuteNonQuery(); 
    }
    finally
    {
        if (connection != null)
            ((IDisposable)connection).Dispose();
    }
}
```

출처: https://hongjinhyeon.tistory.com/92 [생각대로 살지 않으면 사는대로 생각한다.]
