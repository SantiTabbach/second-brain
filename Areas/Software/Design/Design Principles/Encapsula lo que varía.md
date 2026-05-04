---
tags: [areas, software, design, design-principles]
author: Santi Tabbach
created: 2026-05-04
modified: 2026-05-04
---

*“Ide­n­ti­fi­ca los as­pe­c­tos de tu apli­ca­ción que va­rían y se­pá­ra­los de los que se ma­n­tie­nen inalterables.”

**El ob­je­ti­vo pri­n­ci­pal de este pri­n­ci­pio es mi­ni­mi­zar el efe­c­to pro­vo­ca­do por los cambios.**

Pue­des ai­s­lar las pa­r­tes del pro­gra­ma que va­rían, en mó­du­los in­de­pe­n­die­n­tes, pro­te­gie­n­do el resto del có­di­go fre­n­te a efe­c­tos ad­ve­r­sos. Al ha­ce­r­lo, de­di­ca­rás menos tie­m­po a lo­grar que el pro­gra­ma vue­l­va a fu­n­cio­nar, im­ple­me­n­ta­n­do y pro­ba­n­do los ca­m­bios. Cua­n­to menos tie­m­po de­di­ques a rea­li­zar ca­m­bios, más tie­m­po te­n­drás para im­ple­me­n­tar funciones.

---

### En­ca­p­su­la­ción a nivel del mé­to­do

Di­ga­mos que estás crea­n­do un sitio web de co­me­r­cio ele­c­tró­ni­co. En al­gu­na parte de tu có­di­go, hay un mé­to­do obtenerTotaldelPedido que ca­l­cu­la un total del pe­di­do, im­pue­s­tos incluidos.

Po­de­mos an­ti­ci­par que el có­di­go re­la­cio­na­do con los im­pue­s­tos te­n­drá que ca­m­biar en el fu­tu­ro. La tasa im­po­si­ti­va de­pe­n­de­rá de cada país, es­ta­do, o in­clu­so ciu­dad en la que re­si­da el clie­n­te, y la fó­r­mu­la puede va­riar a lo largo del tie­m­po con base a nue­vas leyes o re­gu­la­cio­nes. Esto hará que te­n­gas que ca­m­biar el mé­to­do `obtenerTotaldelPedido` ba­s­ta­n­te a me­nu­do. Pero in­clu­so el no­m­bre del mé­to­do su­gie­re que no le im­po­r­ta cómo se ca­l­cu­la el impuesto.

```typescript
private getOrderTotal(order) {
 let total = 0;
 
 order.lineItems.forEach((item) => {
	total += item.price * item.quantity
	
	if (order.country == "US"){
		total += total * 0.07 // Impuesto sobre la venta de EUA
	} else if (order.country == "EU") {
		total += total * 0.20 // IVA europeo
	}
	    
 });
 
 return total
}
```
*ANTES: el có­di­go de cá­lcu­lo del im­pue­s­to está me­z­cla­do con el resto del có­di­go del método.*

Pue­des ex­traer la ló­gi­ca de cá­lcu­lo del im­pue­s­to a un mé­to­do se­pa­ra­do, es­co­n­dié­n­do­lo del mé­to­do original.
```typescript
private getOrderTotal(order) {
 let total = 0;
 
 order.lineItems.forEach((item) => {
	total += item.price * item.quantity
	total += total * getTaxRate(order.country)
	    
 });
 
 return total
}

private getTaxRate(country) { 
	if (order.country == "US"){
		return 0.07 // Impuesto sobre la venta de EUA
	} else if (order.country == "EU") {
		return 0.20 // IVA europeo
	} else {
		return 0
	}
}
```
*DE­S­PUÉS: pue­des ob­te­ner la tasa im­po­si­ti­va in­vo­ca­n­do un mé­to­do designado.*

Los ca­m­bios re­la­cio­na­dos con el im­pue­s­to que­dan ai­s­la­dos de­n­tro de un único mé­to­do. Ade­más, si la ló­gi­ca de cá­lcu­lo del im­pue­s­to se co­m­pli­ca de­ma­sia­do, ahora es más se­n­ci­llo mo­ve­r­la a una clase separada.

---
### En­ca­p­su­la­ción a nivel de la clase

Con el tie­m­po pue­des aña­dir más y más re­s­po­n­sa­bi­li­da­des a un mé­to­do que solía hacer algo se­n­ci­llo. Estos co­m­po­r­ta­mie­n­tos aña­di­dos sue­len venir con sus pro­pios ca­m­pos y mé­to­dos de ayuda que aca­ban nu­bla­n­do la re­s­po­n­sa­bi­li­dad pri­n­ci­pal de la clase co­n­te­ne­do­ra. Si se ex­trae todo a una nueva clase se puede co­n­se­guir mayor cla­ri­dad y sencillez.

![[Pasted image 20260504233320.png|271]]
*ANTES: cá­lcu­lo del im­pue­s­to en la clase Pedido.*

Los ob­je­tos de la clase `Pedido` de­le­gan todo el tra­ba­jo re­la­cio­na­do con el im­pue­s­to a un ob­je­to es­pe­cial de­di­ca­do justo a eso.

![[Pasted image 20260504233432.png|378]]
*DE­S­PUÉS: el cá­lcu­lo del im­pue­s­to se es­co­n­de de la clase Pedido.*

---

Excerpt From
Sumérgete en los patrones de diseño
Alexander Shvets
This material may be protected by copyright.