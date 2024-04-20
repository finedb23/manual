# Factory Method

## あるサブクラスと別のサブクラスを必ずペアにして使ってもらいたい

```java
abstract class A {
	public abstract String getName();

	public abstract int getAge();

	public abstract B createB();
}

abstract class B {
	public abstract String getAddress();

	public abstract int getFloor();
}

class A1 extends A {

	@Override
	public String getName() {
		return "山田";
	}

	@Override
	public int getAge() {
		return 47;
	}

	@Override
	public B createB() {
		return new B1();
	}
}

class A2 extends A {

	@Override
	public String getName() {
		return "田中";
	}

	@Override
	public int getAge() {
		return 22;
	}

	@Override
	public B createB() {
		return new B2();
	}
}

class B1 extends B {

	@Override
	public String getAddress() {
		return "滋賀";
	}

	@Override
	public int getFloor() {
		return 2;
	}
}

class B2 extends B {

	@Override
	public String getAddress() {
		return "東京";
	}

	@Override
	public int getFloor() {
		return 22;
	}
}
```