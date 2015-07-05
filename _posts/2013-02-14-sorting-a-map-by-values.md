---
title: Sorting a map by values
tags: [java, hibernate]
---

###How to sort a map in Java by its values instead of by its keys.
Most applications of Maps do not require ordering. Occasionally, we stumble upon instances where maps need sorting. To maintain insertion order in Maps, use a [LinkedHashMap](http://docs.oracle.com/javase/6/docs/api/java/util/LinkedHashMap.html). To sort on the keys, use a [SortedMap](http://docs.oracle.com/javase/6/docs/api/java/util/SortedMap.html).

However, once every thousand years or so, the planets align and we need to sort a map by its values. For these situations, some people prefer to create a List of Map entries (List<Map.Entry<K,V>> and then sort using Collections.sort() with a custom Comparator. That works if you don’t need the output as a map. Yes, you can put all the list entries into a map, but let’s assume that the fundamental reason you’re here is that you’re too lazy to write all that code every time you have to sort a map.

A simpler way of sorting a map’s values is to use a SortedMap that sorts a map using a Comparator to compare the Map’s values. The only restriction you have is that the values must implement Comparable – not a sin in our world. Here’s the code...

{% highlight java %}
/**
* Utility to sort a map by values.
*
* @return {@link TreeMap} map sorted by its values
* @throws {@link NullPointerException} if one of the values in the map is
*         null.
*/
public static <K, V extends Comparable<V>> Map<K, V> sortByValue(Map<K, V> unsortedMap) {
  Map<K, V> sortedMap = new TreeMap<K, V>(new ValueComparator<K, V>(unsortedMap));
  sortedMap.putAll(unsortedMap);
  return sortedMap;
}

static class ValueComparator<K, V extends Comparable<V>> implements Comparator<K> {
  private Map<K, V> innerMap;

  private ValueComparator(Map<K, V> innerMap) {
  this.innerMap = innerMap;
}

  @Override
  public int compare(K o1, K o2) {
    V value1 = innerMap.get(o1);
    V value2 = innerMap.get(o2);

  /*
  * If the values are equal, return 1.
  * This is because the sortedMap thinks
  * it's comparing keys and replaces the
  * existing value of the key with the new
  * one if the comparator says they're equal.
  */
  return (value1.compareTo(value2) == 0) ? 1 : value1.compareTo(value2);
  }
}
{% endhighlight %}