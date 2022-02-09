
# VUE.JS V3 NOTLARI

Bu repo vue.js kullanımına ilişkin notlar ve örnekleri içermektedir.

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
Eğer mesajımızın içerisinde html elementi kullanmak istiyorsak bunun için ```v-html``` kullanabiliriz. 

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

### v-on
Bu directive ile tanımlanan bir metod çalıştırılır. (onclick, onchange vb...).

### v-slot
### v-pre
### v-cloak
### v-memo
### v-is

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



Vue komponentinin ilk render edildiği andır.

```bash 
  npm install my-project
  cd my-project
```
    
