
# VUE.JS V3 NOTLARI

Bu repo vue.js kullanımına ilişkin notlar ve örnekleri içermektedir.

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

    
