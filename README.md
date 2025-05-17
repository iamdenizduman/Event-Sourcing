# Event Sourcing

- Bir veri üzerinde meydana gelen tüm değişiklikleri kayıt altında tutmamızı öneren ve böylece ilgili verinin sadece güncel ve ham vaziyetinden ibaret tutulmasından ziyade, o verinin süreçte yaşadığı değişikliklerin de esasında ilgili verinin bir parçası olduğunu savunan ve bundan kaynaklı olarak kayıt altına alınmasını söyleyen bir pattern'dir.
- Örneğin, satrançta her hamlenin türü ve sırası kaydedilir ve mat olduktan sonra o adımları geri aldığımızda başlangıca dönebiliyor olmamız. Her bir hamle bir olaydır. Event, istek neticesinde olayın gerçekleşmesidir. `WhitePawnMoved` bir event'tir.
- Yazılımda bir veri üzerinde gerçekleşen işlemler geri alınabilir ama bu yeni bir event ile gerçekleşir.
- Aggregate, bir veriye ait olabilecek tüm olayları diğer verilerden ayırabilmek için adlandırdığımız tanımdır.

## Özetle

- Event sourcing yalnızca ileriye dönük olmalıdır. Yanlış yazılan yahut geri dönülmesi gereken durumlarda update tarzı işlemler değil, değişikliği ifade edecek olan yeni event'ler oluşturulmalıdır.
- Herhangi bir olayla ilgili tüm veriler, bu veriler için mevcut bir iş ihtiyacı olup olmadığına bakılmaksızın saklanmalıdır. İdeal olarak hiçbir veri atılmamalıdır.

## Kullanım Amaçları

- **Durum geçmişi ve geriye dönük inceleme:** Event sourcing, uygulamanın durum değişikliklerini kaydederek bir geçmiş akışı oluşturur. Böylece uygulamanın geçmiş durumlarına geri dönme ve geçmişteki olayları inceleme yeteneği sağlar.
- **İş analitiği ve raporlama:** Event'ler iş analitiği ve raporlama için zengin bir veri kaynağı sağlar. Uygulamanın davranışları ve kullanıcı etkileşimleri üzerinde detaylı analizler yapılabilir.
- **İş süreçlerinin izlenmesi:** İş süreçlerinin nasıl ilerlediğini izleme konusunda yardımcı olabilir. Event'ler bir iş sürecinin adımlarını ve durumlarını belirlemek için kullanılabilir.
- **Sistem esnekliği:** Farklı modüller arasında haberleşmeyi kolaylaştırabilir. Modüller arasındaki bağlantılar, olaylar aracılığıyla gerçekleşir ve bu da sistemdeki değişikliklere daha iyi adapte olmayı sağlar.
- **Çoğullama ve geçerlilik kontrolü:** Eş zamanlı işlemlerle başa çıkma ve geçerlilik kontrolü sağlama konusunda kullanışlı olabilir. Event'ler uygulama durumunu güncellemek için kullanıldığından, aynı anda gerçekleşen farklı işlemlerin sonuçlarını doğru bir şekilde işleyebilir.
- **Hata kurtarma ve geri alma:** Bir hata oluştuğunda veya istenmeyen bir durum meydana geldiğinde sistem durumunu geri almayı kolaylaştırır. Böylece probleme efektif bir çözüm sağlanabilir.
- Genelde durum geçmişi ve hata kurtarmak için event sourcing kullanırız.

## Özetle

- Event sourcing pattern, verilerin son durumlarını tutmaktan ziyade, verilerin son durumlarını etkileyen olaylar zincirinin kaydedilmesini ve gerektiği takdirde istemci tarafından sorgu gönderilip verinin son durumuna dair sistemdeki var olan tüm event'lerin bilgilerini birleştirerek istemciye sunulmasını önerir.

## Geleneksel Yaklaşım vs Event Sourcing

- Gelenekselde bir durum (state) vardır ve bu durum, veritabanı veya benzer bir depolama mekanizması üzerinde saklanır. State, doğrudan güncellenir ve uygulama bu güncellenmiş durumu kullanır. Event sourcing'de ise durum, geçmişte gerçekleşen olaylardan üretilir. Olaylar, uygulamanın durumundaki değişiklikleri temsil eder ve bu olaylar bir event stream'de saklanır.
- Gelenekselde durum, genellikle tek bir veritabanında veya depolama biriminde saklanır. Uygulama bu durumu doğrudan okuyarak veya yazarak çalışır. Event sourcing'de olaylar genellikle bir olay akışında (event stream) saklanır ve bu event stream, uygulamanın durumunu yeniden oluşturmak için kullanılır.
- Gelenekselde durumun geçmişi tutulmaz veya sınırlı bir şekilde tutulur. Anlık durum, veritabanındaki güncel değeri temsil ederken; sourcing'de olaylar uygulamanın geçmiş durum değişikliklerini temsil eder. Bu nedenle event sourcing ile uygulamanın durum geçmişi takip edilebilir.
- Gelenekselde durum güncellendiğinde geri alma işlemleri genellikle sınırlıdır. Belirli bir durumu geri almak zor olabilir. Sourcing, geri alınabilme ve hata kurtarma işlemlerini kolaylaştırır. Bir hata oluştuğunda veya istenmeyen bir durum meydana geldiğinde sistem durumu belirli bir noktaya geri alınabilir.

## Event Sourcing'in Loglamadan Farkı Nedir?

- Event'ler, uygulamanın durumundaki değişiklikleri temsil ederler. Yani event'ler, genellikle bu durumların akışını takip etmek ve gerektiğinde herhangi bir T zamanda akışın hangi state'te olduğunu görmek için kullanılırlar. Loglama ise genellikle uygulama içindeki olayları ve durumları kaydetmek ve metrik oluşturmak için kullanılır. Event'lere nazaran daha az ayrıntıya sahiptir ve genellikle kullanım amaçları hata ayıklama ve izlemedir.
- Event'ler, uygulamanın geçmiş state değişikliklerini temsil ederler. Bundan dolayı event sourcing ile uygulamanın herhangi bir zamandaki state'i tekrardan oluşturulabilir. Loglar, geçmiş olayların tam bir temsilini sağlamazlar. Genellikle anlık state ve belirli olayların gerçekleşmesine odaklı bilgi içerirler.
- Event'ler, iş analitiği, sistem esnekliği, hata kurtarma ve geri alma gibi amaçlar doğrultusunda tercih edilir. Bunların dışında bir verinin mevcut hale gelebilmesi için süreçte nasıl etkilere maruz kaldığına dair akış oluşturmak amacıyla da kullanılabilirler. Loglama ise genelde izleme, hata ayıklama ve performans analizi gibi amaçlar için kullanılır.
- Sourcing bir design pattern ve mimarisel yaklaşımdır. Loglama ise yardımcı araç ve hizmet olarak kullanılan bir tercihtir.
- Sourcing, ihtiyaç dahilinde veriyi belli bir state'e geri alabilir, hatalı durumu düzeltebilirken; loglamada ise genellikle sistemin runtime'daki reflekslerini izlemek ve hatalarını ayıklamak amacıyla kullanılır, geri alma için kullanılamaz.

## Kaynakça

- Gençay Yıldız - Mikroservis Eğitimleri (YouTube)
