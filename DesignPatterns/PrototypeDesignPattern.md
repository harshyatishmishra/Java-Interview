* Prototype design pattern is used when the Object creation is a costly affair and requires a lot of time and resources and you have a similar object already existing.

* Prototype pattern provides a mechanism to copy the original object to a new object and then modify it according to our needs. Prototype design pattern uses java cloning to copy the object.


```
import java.util.HashMap;
import java.util.Map;

abstract class Stock implements Cloneable{
	protected String name;
	abstract void addStock();
	
	public Object clone() {
		Object clone = null;
		try {
			clone = super.clone();
		} catch (CloneNotSupportedException e) {
			e.printStackTrace();
		}
		return clone;
	}
}
```
```
class Tcs extends Stock{
	Tcs(){
		this.name= "TCS";
	}

	@Override
	void addStock() {
		System.out.println("TCS added");		
	}	
}
```
```
class Infosys extends Stock{
	Infosys(){
		this.name= "Infosys";
	}

	@Override
	void addStock() {
		System.out.println("Infosys added");		
	}	
}
```
```
class StockInventory{
	private static Map<String, Stock> stockMap = new HashMap<>();
	static {
		stockMap.put("TCS", new Tcs());
		stockMap.put("Infosys", new Infosys());
	}
	public static Stock getStock(String name) {
		return (Stock)stockMap.get(name).clone();
	}
}
```
```
public class PrototypePattern {
	
	public static void main(String[] args) {
			Stock stock = StockInventory.getStock("Infosys");
			Stock stock1 = StockInventory.getStock("TCS");
			System.out.println(stock.hashCode());
			System.out.println(stock1.hashCode());
	}

}
```
