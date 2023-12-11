- call by value : 복사 일어남
  ```c++
  void add(int other){
	  this->value_ += other.value_;
	  other.value_++;
}
int main(){
	Int a(2), b(3);
	a.add(b);
}
```
- call by reference
  ```c++
  void add(Int & other){
	this-> value_ += other. value_;
	other.value_++;
}
int main(){
	Int a(2), b(3);
	a.add(b);
}
```
- call by reference  + pointer
  ```c++
  void add(Int * pOther){
	this-> value_ += pOther->value ;
	pOther->value_++;
}
int main(){
	Int a(2), b(3);
	a.add( &b );
}
  ```
- 임시 객체 반환
  ```c++
  Int add(Int one, Int two){
	Int c (one.getValue()+two.getValue());
	return c;
}
```
- 객체 자신 반환
  ```c++
  class Printer {
	public:
	Printer out(string s){
	cout << s;
	return *this; // return this 가 아니다.
	}
};
```
- const 함수에 들어가는 함수는 뒤에 const 붙이기
- const pointer 벗어나기
  ```c++
  void print(const char* const s){
  char* p = (char*)s;
  }
```

[[12050]] 
[[12060]] 
