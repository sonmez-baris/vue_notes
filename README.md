
# VUE.JS V3 NOTLARI

Bu repo vue.js kullanımına ilişkin notlar ve örnekleri içermektedir.

1. [KOMPONENTLER İLE ÇALIŞMA](#komponentler-i̇le-çalişma)
2. [DIRECTIVE (YÖNERGELER)](#directive-y%C3%B6nergeler)
3. [LIFECYCLE METODLAR](#lifecycle-metodlar)

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
