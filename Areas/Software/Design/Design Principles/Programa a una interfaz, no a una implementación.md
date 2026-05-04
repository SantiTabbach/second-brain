---
tags: [areas, software, design, design-principles]
author: Santi Tabbach
created: 2026-05-04
modified: 2026-05-04
---

*"Pro­gra­ma a una in­te­r­faz, no a una im­ple­me­n­ta­ción. De­pe­n­de de ab­s­tra­c­cio­nes, no de cla­ses concretas."*

**Sa­brás que el di­se­ño es lo su­fi­cie­n­te­me­n­te fle­xi­ble si pue­des ex­te­n­de­r­lo con fa­ci­li­dad y sin de­s­co­m­po­ner el có­di­go exi­s­te­n­te.** 

Vamos a ase­gu­rar­nos de que esta afi­r­ma­ción es co­rre­c­ta vie­n­do un eje­m­plo con gatos. Un `Gato` que puede comer cua­l­quier co­mi­da es más fle­xi­ble que uno que sólo puede comer sa­l­chi­chas. Al pri­mer gato le pue­des dar sa­l­chi­chas po­r­que éstas son un su­b­gru­po de “cua­l­quier co­mi­da”, pero pue­des ex­te­n­der el menú de ese gato con cua­l­quier otra comida.

Cua­n­do quie­res hacer que dos cla­ses co­la­bo­ren, pue­des co­me­n­zar por hacer una de­pe­n­die­n­te de la otra. Ca­ra­m­ba, yo mismo suelo em­pe­zar ha­cie­n­do eso. Sin em­ba­r­go, hay otra forma más fle­xi­ble de es­ta­ble­cer la co­la­bo­ra­ción entre objetos:

1. De­te­r­mi­na lo que ne­ce­si­ta exac­ta­me­n­te un ob­je­to del otro: ¿qué mé­to­dos eje­cu­ta?
2. De­s­cri­be esos mé­to­dos en una nueva in­te­r­faz o clase abstracta.
3. Haz que la clase que es una de­pe­n­de­n­cia im­ple­me­n­te esta interfaz.
4. Ahora, haz la se­gu­n­da clase de­pe­n­die­n­te de esta in­te­r­faz en lugar de la clase co­n­cre­ta. To­da­vía pue­des ha­ce­r­lo fu­n­cio­nar con ob­je­tos de la clase ori­gi­nal, pero ahora la co­ne­xión es mucho más flexible.”

![[Pasted image 20260504233923.png|382]]
*Antes y de­s­pués de ex­traer la in­te­r­faz. El có­di­go de la de­re­cha es más fle­xi­ble que el de la iz­quie­r­da, pero ta­m­bién es más complicado.*

#### Ejemplo
Ima­gi­na que estás crea­n­do un si­mu­la­dor de em­pre­sa de de­sa­rro­llo de so­f­t­wa­re. Tie­nes di­s­ti­n­tas cla­ses que re­pre­se­n­tan va­rios tipos de empleados.

![[Pasted image 20260504234126.png|386]]
*ANTES: todos las cla­ses están fue­r­te­me­n­te acopladas.*

Al pri­n­ci­pio, la clase `Empresa` está fue­r­te­me­n­te aco­pla­da a cla­ses de em­plea­dos co­n­cre­tos. Sin em­ba­r­go, a pesar de la di­fe­re­n­cia en sus im­ple­me­n­ta­cio­nes, po­de­mos ge­ne­ra­li­zar va­rios mé­to­dos re­la­cio­na­dos con el tra­ba­jo y ex­traer de­s­pués una in­te­r­faz común para todas las cla­ses de empleado.
De­s­pués de hacer esto, po­de­mos apli­car el po­li­mo­r­fi­s­mo de­n­tro de la clase `Empresa`, tra­ta­n­do va­rios ob­je­tos de em­plea­do a tra­vés de la in­te­r­faz `Empleado`.

![[Pasted image 20260504234307.png|450]]
*MEJOR: el po­li­mo­r­fi­s­mo nos ayudó a si­m­pli­fi­car el có­di­go, pero el resto de la clase Empresa aún de­pe­n­de de las cla­ses de em­plea­dos concretos.*

La clase `Empresa` sigue aco­pla­da a las cla­ses de em­plea­do. Esto no es bueno, po­r­que si in­tro­du­ci­mos nue­vos tipos de em­pre­sas que fu­n­cio­nan con otros tipos de em­plea­dos, ne­ce­si­ta­re­mos so­bre­s­cri­bir la mayor parte de la clase `Empresa` en lugar de re­uti­li­zar ese código.
Para re­so­l­ver este pro­ble­ma, po­de­mos de­cla­rar el mé­to­do para ob­te­ner em­plea­dos como ab­s­tra­c­tos. Cada em­pre­sa co­n­cre­ta im­ple­me­n­ta­rá este mé­to­do de forma di­fe­re­n­te, crea­n­do úni­ca­me­n­te los em­plea­dos que necesita.

![[Pasted image 20260504234510.png|526]]
*DE­S­PUÉS: el mé­to­do pri­ma­rio de la clase `Empresa` es in­de­pe­n­die­n­te de cla­ses de em­plea­do co­n­cre­tos. Los ob­je­tos de em­plea­do se crean en su­b­cla­ses de em­pre­sas concretas.*

Tras este ca­m­bio, la clase `Empresa` se vue­l­ve in­de­pe­n­die­n­te de va­rias cla­ses de em­plea­do. Ahora pue­des ex­te­n­der esta clase e in­tro­du­cir nue­vos tipos de em­pre­sas y em­plea­dos, a la vez que re­uti­li­zas una parte de la clase base em­pre­sa. Ex­te­n­der la clase base em­pre­sa no de­s­co­m­po­ne el có­di­go exi­s­te­n­te que ya se basa en ella.

---

Excerpt From
Sumérgete en los patrones de diseño
Alexander Shvets
This material may be protected by copyright.