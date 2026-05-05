---
tags: [areas, software, design, design-principles]
author: Santi Tabbach
created: 2026-05-04
modified: 2026-05-05
---

La he­re­n­cia es pro­ba­ble­me­n­te la forma más obvia y se­n­ci­lla de re­uti­li­zar có­di­go entre cla­ses. Tie­nes dos cla­ses con el mismo có­di­go. Creas una clase base común para estas dos cla­ses y co­lo­cas de­n­tro el có­di­go si­mi­lar. ¡Pan comido!

La­me­n­ta­ble­me­n­te, la he­re­n­cia tiene sus co­n­tras, que a me­nu­do no re­su­l­tan apa­re­n­tes hasta que tu pro­gra­ma tiene to­ne­la­das de cla­ses y ca­m­biar­lo todo re­su­l­ta muy co­m­pli­ca­do. Aquí tie­nes una lista de esos problemas.

- Una su­b­cla­se no puede re­du­cir la in­te­r­faz de la su­pe­r­cla­se. Tie­nes que im­ple­me­n­tar todos los mé­to­dos ab­s­tra­c­tos de la clase padre, in­clu­so au­n­que no vayas a usarlos.
- Al so­bre­s­cri­bir mé­to­dos debes ase­gu­rar­te de que el nuevo co­m­po­r­ta­mie­n­to sea co­m­pa­ti­ble con el de base. Es im­po­r­ta­n­te po­r­que los ob­je­tos de la su­b­cla­se pue­den pa­sar­se a cua­l­quier có­di­go que es­pe­re ob­je­tos de la su­pe­r­cla­se y no quie­res que ese có­di­go se rompa.
- La he­re­n­cia rompe la en­ca­p­su­la­ción de la su­pe­r­cla­se po­r­que los de­ta­lles in­te­r­nos de la clase padre se hacen di­s­po­ni­bles para la su­b­cla­se. Puede darse una si­tua­ción opue­s­ta en la que un pro­gra­ma­dor hace que una su­pe­r­cla­se co­no­z­ca al­gu­nos de­ta­lles de las su­b­cla­ses, con el ob­je­ti­vo de que las si­guie­n­tes ex­te­n­sio­nes sean más sencillas.
- Las su­b­cla­ses están fue­r­te­me­n­te aco­pla­das a su­pe­r­cla­ses. Cua­l­quier ca­m­bio en una su­pe­r­cla­se puede de­s­co­m­po­ner la fu­n­cio­na­li­dad de las subclases.
- In­te­n­tar re­uti­li­zar có­di­go me­dia­n­te la he­re­n­cia puede co­n­du­cir a la crea­ción de je­ra­r­quías de he­re­n­cia pa­ra­le­las. No­r­ma­l­me­n­te, la he­re­n­cia su­ce­de en una única di­me­n­sión. Pero cua­n­do hay dos o más di­me­n­sio­nes, debes crear mu­chas co­m­bi­na­cio­nes de cla­ses, hi­n­cha­n­do la je­ra­r­quía de cla­ses hasta un ta­ma­ño ridículo.

Exi­s­te una al­te­r­na­ti­va a la he­re­n­cia lla­ma­da co­m­po­si­ción. Mie­n­tras que la he­re­n­cia re­pre­se­n­ta la re­la­ción “is a” (es un/a) entre cla­ses (un auto es un medio de tra­n­s­po­r­te), la co­m­po­si­ción se basa en la re­la­ción “tiene un/a” (un auto tiene un motor).

Debo me­n­cio­nar que este pri­n­ci­pio ta­m­bién se apli­ca a la agre­ga­ción, una va­ria­n­te más re­la­ja­da de la co­m­po­si­ción en la que un ob­je­to puede co­n­te­ner una re­fe­re­n­cia al otro pero no ge­s­tio­na su ciclo vital. Aquí tie­nes un eje­m­plo: un auto tiene un co­n­du­c­tor pero éste puede uti­li­zar otro auto o ca­mi­nar sin el auto.

#### Eje­m­plo
Ima­gi­na que debes crear una apli­ca­ción de un ca­tá­lo­go para un fa­bri­ca­n­te de au­to­mó­vi­les. La em­pre­sa fa­bri­ca autos y ca­mio­nes; pue­den ser elé­c­tri­cos o de ga­so­li­na; todos los mo­de­los pue­den tener co­n­tro­les ma­nua­les o pi­lo­to automático.

![[Pasted image 20260504235852.png|546]]
*HE­RE­N­CIA: ex­te­n­der una clase en va­rias di­me­n­sio­nes (tipo de carga × tipo de motor × tipo de na­ve­ga­ción) puede pro­vo­car una ex­plo­sión co­m­bi­na­to­ria de subclases.*

Como pue­des ver, cada pa­rá­me­tro adi­cio­nal re­su­l­ta en la mu­l­ti­pli­ca­ción del nú­me­ro de su­b­cla­ses. Hay mucho có­di­go du­pli­ca­do entre su­b­cla­ses po­r­que una su­b­cla­se no puede ex­te­n­der dos cla­ses al mismo tiempo.

Pue­des re­so­l­ver este pro­ble­ma con la co­m­po­si­ción. En lugar de que los ob­je­tos de auto im­ple­me­n­ten un co­m­po­r­ta­mie­n­to por su cue­n­ta, pue­den de­le­gar­lo a otros objetos.

La ve­n­ta­ja adi­cio­nal es que pue­des su­s­ti­tuir un co­m­po­r­ta­mie­n­to du­ra­n­te el tie­m­po de eje­cu­ción. Por eje­m­plo, pue­des su­s­ti­tuir un ob­je­to de motor vi­n­cu­la­do a un ob­je­to de auto asi­g­na­n­do si­m­ple­me­n­te un ob­je­to de motor di­s­ti­n­to al auto.

![[Pasted image 20260505000043.png|341]]
*CO­M­PO­SI­CIÓN: di­fe­re­n­tes “di­me­n­sio­nes” de fu­n­cio­na­li­dad ex­traí­das para po­ne­r­se de­n­tro de sus pro­pias je­ra­r­quías de clases..*

---

Excerpt From
Sumérgete en los patrones de diseño
Alexander Shvets
This material may be protected by copyright.