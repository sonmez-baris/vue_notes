
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
Süslü parantezler kullanmak yerine “v-text” Directive’ini kullanarak da mesajımızı ekranda gösterebiliriz.

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
Eğer mesajımızın içerisinde html elementi kullanmak istiyorsak bunun için v-html kullanabiliriz. 

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
### v-once
### v-bind
### v-if
### v-else-if
### v-else
### v-show
### v-for
### v-on
### v-text


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
    
