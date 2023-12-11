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
