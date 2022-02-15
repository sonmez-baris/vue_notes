## Description

![https://img.shields.io/badge/Vue.js->=3-41B883](https://img.shields.io/badge/Vue.js->=3-41B883) ![https://img.shields.io/github/last-commit/sonmez-baris/vue_notes](https://img.shields.io/github/last-commit/sonmez-baris/vue_notes)

This repository has Turkish notes, translates, collections, quotes and examples about vue3. It is not yet completed and is being updated.

# VUE.JS V3 NOTLARI

1. [KOMPONENTLER İLE ÇALIŞMA](#komponentler-i̇le-çalişma)
2. [DIRECTIVE (YÖNERGELER)](#directive-y%C3%B6nergeler)
3. [LIFECYCLE METODLAR](#lifecycle-metodlar)
4. [REAKTİVİTENİN TEMELLERİ](#reaktivitenin-temelleri)
5. [COMPUTED PROPERTIES AND WATCHERS](#computed-properties-and-watchers)
6. [SINIF VE STİL BAĞLANTILARI](#sinif-ve-sti%CC%87l-ba%C4%9Flantilari)

## KOMPONENTLER İLE ÇALIŞMA
Bileşen sistemi, Vue'daki bir diğer önemli kavramdır, çünkü küçük, bağımsız ve genellikle yeniden kullanılabilir bileşenlerden oluşan büyük ölçekli uygulamalar oluşturmamıza izin veren bir soyutlamadır. Bunu düşünürsek, hemen hemen her tür uygulama arayüzü bir bileşen ağacına soyutlanabilir:
Vue'da bir bileşen, esasen önceden tanımlanmış seçeneklere sahip bir örnektir. Vue'da bir bileşeni kaydetmek basittir: Bir bileşen nesnesi yaratırız ve onu ebeveynine tanımlarız:

```bash 
const TodoItem = {
  template: `<li>This is a todo</li>`
}

// Create Vue application
const app = Vue.createApp({
  components: {
    TodoItem // Register a new component
  },
  ... // Other properties for the component
})

// Mount Vue application
app.mount(...)
```

Artık onu başka bir bileşenin şablonunda oluşturabilirsiniz:

```bash 
<ol>
  <!-- Create an instance of the todo-item component -->
  <todo-item></todo-item>
</ol>
```

Ancak bu, her yapılacak iş için aynı metni oluşturur, bu da çok kullanışlı değildir. Ana kapsamdaki verileri alt bileşenlere aktarabilmeliyiz. Bir prop kabul etmesi için bileşen tanımını değiştirelim :

const TodoItem = {
  props: ['todo'],
  template: `<li>{{ todo.text }}</li>`
}

Şimdi, yapılacakları her tekrarlanan alt bileşenlere v-bind ile iletebiliriz:

```bash 
<div id="todo-list-app">
  <ol>
    <!--
      Now we provide each todo-item with the todo object
      it's representing, so that its content can be dynamic.
      We also need to provide each component with a "key",
      which will be explained later.
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>
```

```bash 
const TodoItem = {
  props: ['todo'],
  template: `<li>{{ todo.text }}</li>`
}

const TodoList = {
  data() {
    return {
      groceryList: [
        { id: 0, text: 'Vegetables' },
        { id: 1, text: 'Cheese' },
        { id: 2, text: 'Whatever else humans are supposed to eat' }
      ]
    }
  },
  components: {
    TodoItem
  }
}

const app = Vue.createApp(TodoList)

app.mount('#todo-list-app')
```

## DIRECTIVE (YÖNERGELER)
Yönergelerin önüne ```v-``` Vue tarafından sağlanan özel nitelikler olduklarını belirtmek için eklenir ve oluşturulan DOM'a özel reaktif davranış uygularlar.

### mustache
```mustache``` ile datada tuttuğumuz bir veriyi ekrana bastırabiliriz. Bunun için süslü parantez kullanılır.
ÖR;

```bash 
<p>Using mustaches: {{ rawHtml }}</p>
```

Burada unutulmaması gereken süslü parantezler ile HTML etiketlerini bastıramadığımız.

### v-text
Süslü parantezler kullanmak yerine ```v-text``` Directive’ini kullanarak da mesajımızı ekranda gösterebiliriz.

```bash 
<p v-text="msg">...</p>

<script>
export default {
    name: 'TopBar',
    props: {
        msg: String
    }
};
</script>
```

### v-html
Eğer mesajımızın içerisinde html elementi kullanmak istiyorsak bunun için ```v-html``` kullanabiliriz. İçeriğin düz HTML olarak eklendiğini unutmayın; bunlar Vue şablonları olarak derlenmeyecektir.

```bash 
<div id="example1" class="demo">
  <p>Using mustaches: {{ rawHtml }}</p> //HTML kodları ekrana basılır.
  <p>Using v-html directive: <span v-html="rawHtml"></span></p> //HTML kodları işlenir.
</div>

<script>
export default {
  data() {
    return {
      rawHtml: '<span style="color: red">This should be red.</span>'
    }
  }
};
</script>
```

### v-model
Bu directive ile form input'a ait değeri data ile bağlayabilmekteyiz. Forma girilen value böylelikle data'ya taşınır.

```bash 
<input type="text" v-model="msg">
<p>{{ msg }}</p>

<script>
export default {
  data() {
    return {
      msg: 'Hello Vue'
    }
  }
};
</script>
```

```bash 
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no" />
```

```true-value``` ve ```false-value``` yalnızca ile çalışan Vue'ya özgü niteliklerdir. Burada toggle özelliğin değeri 'yes', kutu işaretlendiğinde ve 'no' işaretlenmediğinde ayarlanır. Bunları aşağıdakileri kullanarak ```v-bind``` dinamik değerlere de bağlayabilirsiniz:

```bash 
<input
  type="checkbox"
  v-model="toggle"
  :true-value="dynamicTrueValue"
  :false-value="dynamicFalseValue" />
```

### v-once
Bu directive ile bir veriyi sabit tutabiliriz. Yani datayı ekrana yazdırdıktan sonra, data değişse bile “v-once” kullandığımız için ekranda görünen mesaj sabit kalacaktır.

```bash 
<input type="text" v-model="msg">
<p v-once="msg"></p>

<script>
export default {
  data() {
    return {
      msg: 'Hello Vue'
    }
  }
};
</script>
```

### v-bind
Bu directive ile attribute'lar bind edilir. Kullanıma dair örnek aşağıdadır;

```bash 
<a v-bind:href="url" v-bind:title="title">{{ title }}</a>
<a :href="url" :title="title">{{ title }}</a>

<script>
export default {
  data() {
    return {
      url: 'https://www.barissonmez.com.tr',
      title: 'Barış Sönmez',
    }
  }
};
</script>
```

Ayrıca ```v-bind``` yerine kısa kullanım olarak ```:``` da attribute'lar önünde kullanılır.

```bash 
<!-- Bir attr bağla -->
<img v-bind:src="imageSrc" />

<!-- Dinamik bir attr -->
<button v-bind:[key]="value"></button>

<!-- Kısa kullanım -->
<img :src="imageSrc" />

<!-- Dinamik bir attr için kısa kullanım -->
<button :[key]="value"></button>

<!-- Satır içinde + parametresi ile dize birleştirme -->
<img :src="'/path/to/images/' + fileName" />

<!-- class bağlama -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]"></div>

<!-- style bağlama -->
<div :style="{ fontSize: size + 'px' }"></div>
<div :style="[styleObjectA, styleObjectB]"></div>

<!-- Bir attr nesnesini bağlama -->
<div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

<!-- prop bağlama. "prop" komponentlerim içinde bildirilmelidir. -->
<my-component :prop="someThing"></my-component>

<!-- Ana propları, child ile ortak kullanmak -->
<child-component v-bind="$props"></child-component>

<!-- XLink -->
<svg><a :xlink:special="foo"></a></svg>
```

***Boolean Öznitelikleri***
Boolean öznitelikleri , bir öğe üzerindeki varlığına göre doğru/yanlış değerleri gösterebilen özniteliklerdir. Örneğin, disableden sık kullanılan boole özniteliklerinden biridir.

```v-bind``` bu durumda biraz farklı çalışır:

```bash 
<button :disabled="isButtonDisabled">Button</button>
```

Öznitelik , doğruluk değerine sahipse disabled dahil edilir. Yoksa dahil edilmez. 

***JavaScript İfadelerini Kullanma***
Şablonlara basit özellik anahtarları ile bağlanmak dışında, tüm veri bağlamalarında JavaScript ifadelerinin tam gücü de desteklemektedir:

```bash 
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div :id="`list-${id}`"></div>
```

Bu ifadeler, geçerli bileşen örneğinin veri kapsamında JavaScript olarak değerlendirilecektir.

Vue şablonlarında JavaScript ifadeleri süslü parantez içinde ve v- directive'ler de kullanılabilir.

***Modifiers (Değiştiriciler)***

Değiştiriciler, bir yönergenin özel bir şekilde bağlanması gerektiğini belirten, nokta ile gösterilen özel son eklerdir. Örneğin, değiştirici, ```.prevent``` ile yönergeye tetiklenen olayı çağırmasını söyler: ```:v-onevent.preventDefault()```

```bash 
<form @submit.prevent="onSubmit">...</form>
```


### v-if, v-else-if, v-else
Belirlenen koşulun sağlanıp sağlanmama durumuna göre DOM üzerinde değişiklik yapmayı mümkün kılar.

```bash 
<p v-if="1 > 4">{{ condition1 }}</>
<p v-else-if="3 > 4">{{ condition2 }}</>
<p v-else>{{ condition3 }}</>

<script>
export default {
  data() {
    return {
      condition1: 'Mesajı 1',
      condition2: 'Mesajı 2',
      condition3: 'Mesajı 3',
    }
  }
};
</script>
```

### v-show
Bu directive ile bir element show/hide edilebilir. Eğer ```v-show``` değeri “true” ise element gösterilir. Eğer “false” ise elementin “display” özelliğini “none” yaparak ekranda görünmesini engellenir. ```v-if``` ile farkı ise, ```v-show``` 'da element dom üzerinde yaratılır, sadece css özelliği ile gizlenir. ```v-if``` ise elementi dom üzerinden direk kaldırır.

```bash 
<p v-show="1 > 4">{{ msg }}</>

<script>
export default {
  data() {
    return {
      msg: 'Hello Vue!',
    }
  }
};
</script>
```

### v-for
Bir array içerisindeki elemanları DOM üzerine sıra ile basmak için kullanılır. 

```bash 
<ul>
  <li v-for="todo in todos">{{ todo }}</li>
</ul>

<script>
export default {
  data() {
    return {
      todos: ['todo1', 'todo2', 'todo3'],
    }
  }
};
</script>
```

Alternatif olarak, index (veya bir objed kullanılıyorsa key) için bir takma ad da belirtebilirsiniz:

```bash 
<div v-for="(item, index) in items"></div>
<div v-for="(value, key) in object"></div>
<div v-for="(value, name, index) in object"></div>
```

Varsayılan davranışı ```v-for```, öğeleri hareket ettirmeden yerinde düzeltmeye çalışır. Öğeleri yeniden sıralamaya zorlamak için ```:key``` özel öznitelikle bir sıralama ipucu sağlamalısınız:

```bash 
<div v-for="item in items" :key="item.id">
  {{ item.text }}
</div>
```

Örnekler;

```bash 
<ul id="v-for-object" class="demo">
  <li v-for="(value, name) in myObject">
    {{ name }}: {{ value }}
  </li>
</ul>

Vue.createApp({
  data() {
    return {
      myObject: {
        title: 'How to do lists in Vue',
        author: 'Jane Doe',
        publishedAt: '2020-03-22'
      }
    }
  }
}).mount('#v-for-object')
```

```bash 
<li v-for="(value, name, index) in myObject">
  {{ index }}. {{ name }}: {{ value }}
</li>

Vue.createApp({
  data() {
    return {
      myObject: {
        title: 'How to do lists in Vue',
        author: 'Jane Doe',
        publishedAt: '2020-03-22'
      }
    }
  }
}).mount('#v-for-object')
```

**Filtrelenmiş/Sıralanmış Sonuçları Görüntüleme**

Bazen, orijinal verileri gerçekten değiştirmeden veya sıfırlamadan bir dizinin filtrelenmiş veya sıralanmış bir sürümünü görüntülemek isteriz. Bu durumda, filtrelenmiş veya sıralanmış diziyi döndüren hesaplanmış bir özellik oluşturabilirsiniz.

```bash 
<li v-for="n in evenNumbers" :key="n">{{ n }}</li>
data() {
  return {
    numbers: [ 1, 2, 3, 4, 5 ]
  }
},
computed: {
  evenNumbers() {
    return this.numbers.filter(number => number % 2 === 0)
  }
}
```
**ÖNEMLİ UYARI**

```v-if``` ve ```v-for```'un birlikte kullanılmasının önerilmediğini unutmayın. 

Aynı alanda var olduklarında ```v-if```, ```v-for```'dan daha yüksek önceliğe sahiptir. Bu, ```v-if``` koşulunun ```v-for``` kapsamındaki değişkenlere erişimi olmayacağı anlamına gelir. Çözüm ve örnek kullanım için [TIKLAYINIZ](https://v3.vuejs.org/guide/list.html#v-for-with-v-if).

### v-on
Bu directive ile tanımlanan bir metod çalıştırılır. (onclick, onchange vb...).

### v-slot
Komponentlerin içeriklerinin dinamik olarak değiştiği durumlarda ve birden fazla elementin aynı anda gönderilmesi veya component içindeki elementlerin sadece bazılarının aynı anda gönderilmesi gibi işlemlerde slot kullanılır. Kısaca # ile ifade edilebilir.

```bash 
<base-layout>
  <template v-slot:header>
    <h1>H1 Heading</h1>
  </template>

  <p>A paragraph.</p>

  <template #:footer>
    <p>A footer info.</p>
  </template>
</base-layout>
```

### v-pre
Empty directive olarak ifade edilebilir. Herhangi bir expression almaz. v-pre kendi elementi ve alt elementler için derlemenin (compilation) es geçilmesini sağlar. Aşağıdaki gibi bir kullanımda mustaches içeriği uygulanmayacak ve ifade olduğu haliyle tutulacaktır.

```bash 
<p v-pre>{{ selected }}</p>
```

### v-cloak
Bir diğer empty directive olan v-cloak ViewModel derlemeyi tamamlayana kadar eklendiği element üzerinde kalır. Bir CSS tanımı ile birlikte (örneğin bir div için) derleme süreci tamamlanana, ViewModel hazır olana kadar mustaches ifadelerin gizlenmesi sağlanabilir.

```bash 
[v-cloak] {
 display:none
}
```

### v-memo
Template'in sub tree'sini hafızaya alır. Hem elemanlarda hem de komponentlerde kullanılabilir. Bu directive, hafızaya alınan bağlılık değerlerini karşılaştırmak için sabit uzunlukta bir dizi bekler. Dizideki her değer son oluşturma ile aynıysa, tüm sub-tree için güncellemeler atlanır. Örneğin:

```bash 
<div v-memo="[valueA, valueB]">
  ...
</div>
```

### v-is
Vue 3.1.0 ile kullanımdan kaldırıldı.

## LIFECYCLE METODLAR

![Uygulama Ekran Görüntüsü](https://v3.vuejs.org/images/lifecycle.svg)


### beforeCreate
Bu hook metodu, örnek başlatıldıktan hemen sonra, observation ve event/watcher setup'dan önce eşzamanlı olarak çağrılır.

### created
Bu hook metodu, örnek oluşturulduktan sonra eşzamanlı olarak çağrılır. Bu aşamada, data observation (reactivity), events, computed properties ve watchers’lar ayarlanmıştır ve etkileşime girebilir. Fakat mounted olmadığı için DOM ile etkileşime girilmez.

### beforeMount
Mount başlamadan hemen önce çağrılır. Örnek henüz sanal DOM'a yerleştirilmemiştir.

### mounted
Örnek mount işlemi tamamlandıktan sonra çağrılır. Vue.js özel ```app.mount``` yeni oluşturulan ```vm.$el``` e devredilmiştir. Ayrıca artık tüm componentler etkileşime hazır vaziyettedir.
```mounted``` Tüm alt bileşenlerin de mount edildiğini garanti etmez. Tüm görünüm oluşturulana kadar beklemek istiyorsanız, içinde ```vm.$nextTick``` kullanabilirsiniz.

```bash 
mounted() {
  this.$nextTick(function () {
    // Code that will run only after the
    // entire view has been rendered
  })
}
```

### beforeUpdate
### updated	
### beforeUnmount
### unmounted
### errorCaptured
### renderTracked
### renderTriggered
### activated
### deactivated

## REAKTİVİTENİN TEMELLERİ

Bir bileşen örneğine yöntemler eklemek için methodsseçeneği kullanırız. Bu, istenen yöntemleri içeren bir nesne olmalıdır:

```bash 
<button @click="increment">{{ count }}</button>

<script>
export default {
  data() {
    return {
      count: 0
    }
  },
  methods: {
    increment() {
      this.count++
    }
  },
  mounted() {
    // methods can be called in lifecycle hooks, or other methods!
    this.increment()
  }
}
</script>
```

***DOM Güncelleme Zamanlaması***

!!! Reaktif durumu değiştirdiğinizde, DOM otomatik olarak güncellenir. Ancak, DOM güncellemelerinin eşzamanlı olarak uygulanmadığına dikkat edilmelidir. Bunun yerine, Vue, yaptığınız durum değişiklikleri ne olursa olsun her bileşenin yalnızca bir kez güncellenmesi gerektiğinden emin olmak için güncelleme döngüsündeki "bir sonraki onay işaretine" kadar arabelleğe alır.

Bir durum değişikliğinden sonra DOM güncellemesinin tamamlanmasını beklemek için ```nextTick()``` global API'sini kullanabilirsiniz:

```bash
import { nextTick } from 'vue'

export default {
  methods: {
    increment() {
      this.count++
      nextTick(() => {
        // access updated DOM
      })
    }
  }
}
```

***Derin Reaktivite***

Vue'da durum varsayılan olarak derinden reaktiftir. Bu, iç içe geçmiş nesneleri veya dizileri değiştirdiğinizde bile değişikliklerin algılanmasını bekleyebileceğiniz anlamına gelir:

```bash
export default {
  data() {
    return {
      obj: {
        nested: { count: 0 },
        arr: ['foo', 'bar']
      }
    }
  },
  methods: {
    mutateDeeply() {
      // these will work as expected.
      this.obj.nested.count++
      this.obj.arr.push('baz')
    }
  }
}
```

## COMPUTED PROPERTIES AND WATCHERS

Vue Lifecycle dahilinde yer alan computed, render işlemi tamamlandıktan sonraki andır ve computed adında bir metot oluşturup kullanılır. 

```bash 
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

Yukarıdaki kullanımda, message alındığında bir dizi işlemden geçirilmekte; ```split()``` ile parçalara ayrılmakta, ```reverse()``` ile karakterlerin yerleri değiştirilmekte ve ```join()``` ile yerleri değiştirilen karakterler birleştirilmekte. Sonuç olarak, message tanımı, örneğin ```data { message: 'Hello' }``` olsun, ```olleH``` haline getirilerek ekranda gösterilmekte. Kapsamlı bir yapı içerisinde, template oluştururken veya düzenlerken bu ve benzeri kullanımlar kontrolü güç bir hale gelebilir. Bu tür durumlarda, gerekli işlemler computed içerisinde kolaylıkla tanımlanabilirler. Aynı işlemi şimdi yeniden ele alalım.

```bash 
const app = {
  data() {
    return {
      message: 'Hello'
    }
  },
  computed: {
    reversedMessage() {
      return this.message.split('').reverse().join('');
    }
  }
}
Vue.createApp(app).mount('#message');
```

Çıktımızın aynı olmasına karşın, önceki kullanımdan farklı olarak HTML etiketi içerisinde ```message``` yerine ```reversedMessage``` kullanmamız gerekmekte. Bu sayede, artık veri değiştikçe ilgili alanın da değiştiğini görebiliriz.

Computed property ile template içerisinde normal property işlemlerinde olduğu gibi data-bind işlemleri yapmak mümkün. Çünkü, Vue örnek üzerinden anlatmak gerekirse, computed olarak tanımlı reversedMessage ile message öğesi birbirine bağlı. Bu sayede message değiştiğinde reversedMessage ve ona bağlı olan tüm alanlar da güncellenir. Bu bağımlılık bildirimsel olarak (declaratively) gerçekleştirilir.

Computed property varsayılan olarak yalnızca getter tanımlıdır, ancak ihtiyacınız olduğunda setter olarak da tanımlanabilir: ```{ get: Function, set: Function }```

```bash 
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```

Console üzerinden ```fullName = 'John Doe'``` tanımı yaptığımızda setter ```firstName``` ve ```lastName``` için de güncelleme geçecektir.

### Computed Property İle Methods Karşılaştırması
v-model direktifi açıklamasında da bahsi geçtiği üzere computed ve methods‘un işlev anlamında örtüştüğü noktalar olsalar da temel ve aslında önemli bir farklılık da mevcut5. computed property bağlı olduğu değişkeni ekranda sunarken ön belleğe (cache) de alır ve bu değişken değişmediği sürece tekrar hesaplama yapmaz. method kullanımında ise hesaplama yinelenir ve son değer ön bellekleme olmadan ekrana yansıtılır. Özetle, method re-render gerçekleşen her durumda yeniden çalıştırılır, ancak computed kapsamındaki işlemler yinelenmez.

```bash 
<p>{{ reverseMessage() }}</p>
```

```bash 
methods: {
  reverseMessage() {
    return this.message.split('').reverse().join('')
  }
}
```
Tanımlandığı alan dışında, yazımda herhangi bir farklılık olmadı ve ekrana yansıyan ifade de aynı.

### Computed Property İle Watched Property Karşılaştırması
VueJS, data değişikliklerini gözlemlemek ve bunlara tepki vermek için watch properties olarak ifade edilen daha genel bir yola daha sahiptir. Lifecycle içerisinde watch component var olduğu sürede, o component içeriğindeki datalarla ilgili değişiklikleri yakalamızı sağlar. Örneğin, bir veri bir başka veriye bağlı ise ve bağlı olduğu veride değişiklik söz konusu olmuşsa watch (watcher / watched prop) kullanımı tercih edilebilir. Şöyle bir örneğimiz olsun;

```bash 
<div id="app">
 <p v-html="welcome"></p>
 <ul>
  <li>Name: <strong>{{ firstName }}</strong></li>
  <li>Surname: <strong>{{ lastName }}</strong></li>
  <li>Full Name: <strong>{{ fullName }}</strong></li>
 </ul>
</div>

<script>
const app = Vue.createApp({
  data() {
    return {
      message: `It's a New Day!`,
      firstName: 'John',
      lastName: 'Doe',
      fullName: 'John Doe'
    }
  },
  computed: {
    welcome() {
      return 'Hello' + ' <strong>' + this.fullName + '</strong>, ' + this.message
    }
  },
  watch: {
    message(val) {
      this.message = val
    },
    firstName(val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName(val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
}).mount('#app');
</script>
```

Console üzerinden app.lastName = 'Wick' tanımı yapalım. Bu durumda hem fullName hem de message içeriğinin değiştiğini görebilirsiniz. Watch alanını temizleyip aynı işlemi gerçekleştirdiğinizde sadece ilgili veri alanı güncellenecektir.

Özetlemek gerekirse, computed properties diğer verilerden türetilmiş yeni veriler oluşturmak istendiğinde öne çıkmaktadır7. Bu verileri dönüştürmek (transform), filtrelemek (filter) ya da değiştirmek (manipulate) istediğimizde rahatlıkla kullanabiliriz. Computed properties her zaman bir değer döndürmek (return) ve eş zamanlı olmak durumundadır. Diğer yandan, bir bileşenin (component) prop aldığını ve bu prop içeriğinin her değişmesi durumunda bir AJAX isteği gerçekleştirilmesi gerektiğini düşünün. Bu durumda Watch property ile verideki değişikliği izlemek çok daha isabetli bir karar olacaktır.


## SINIF VE STİL BAĞLANTILARI
Data bağlama için yaygın bir ihtiyaç, bir öğenin class yapısı ve onun satır içi style değerlerini değiştirmektir. Her ikisi de birer attr olduğundan, ```v-bind``` bunları işlemek için kullanılabilir. 

Sınıfları dinamik olarak değiştirmek için bir nesneyi :class'a (v-bind:class'ın kısaltması) iletebiliriz:

```bash 
<div :class="{ active: isActive }"></div>
```

Nesnede daha fazla alan bulundurarak birden çok sınıf arasında geçiş yapabilirsiniz. Ayrıca, :class yönergesi, düz class niteliği ile birlikte var olabilir;

```bash 
<div
  class="static"
  :class="{ active: isActive, 'text-danger': hasError }"
></div>

data() {
  return {
    isActive: true,
    hasError: false
  }
}
```

Sonuç olarak şöyle bir çıktı alınır;

```html 
<div class="static active"></div>
```

```isActive``` veya ```hasError``` değiştiğinde, sınıf listesi buna göre güncellenecektir. Örneğin, ```hasError``` true olursa, sınıf listesi ```statik active text-danger``` olur.

**Ayrıca bağlı nesnenin doğrudan DOM üzerinde tanımlanması gerekmez.**

```bash 
<div :class="classObject"></div>

data() {
  return {
    classObject: {
      active: true,
      'text-danger': false
    }
  }
}
```

Bu aynı sonucu verecektir. Ayrıca bir nesne döndüren hesaplanmış bir özelliğe de (computed property) bağlanabiliriz. Bu yaygın ve güçlü bir kalıptır:

```bash 
data() {
  return {
    isActive: true,
    error: null
  }
},
computed: {
  classObject() {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

Bir sınıf listesini uygulamak için bir diziyi :class öğesine iletebiliriz:

```bash 
<div :class="[activeClass, errorClass]"></div>

data() {
  return {
    activeClass: 'active',
    errorClass: 'text-danger'
  }
}
```

### style kullanımı
Style kullanımı da son derece basittir. JS nesnesi olması dışında tamamen CSS yapısı hakimdir.

```bash 
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>

data() {
  return {
    activeColor: 'red',
    fontSize: 30
  }
}
```

class bağlarken kullandığımız birçok yöntem style içinde geçerlidir.

Çoklu style değerleri ile çalışırken;

```bash 
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

## KAYNAKÇA

- [https://v3.vuejs.org/guide/](https://v3.vuejs.org/guide/)
- [https://ceaksan.com/](https://ceaksan.com/)
