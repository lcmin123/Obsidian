- call by value : 복사 일어남
  ```c++
  void add(int other){
	  this->value_ += other.value_;
	  other.value_++;
}
```
- call by re