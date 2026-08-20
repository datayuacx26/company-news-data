---
schema_version: "1.0.0"
document_id: "bcb164d9bf63b220690b02800b90fd2178f44f088df3b05ee4282bb38b851230"
company_key: "proto-labs-inc-common-stock"
company: "Proto Labs Inc."
source_id: "proto-labs-inc-common-stock-news-import-1be3061a2e2a"
canonical_url: "https://www.protolabs.com/fr-fr/ressources/blog/considerations-sur-l-orientation-des-pieces-imprimees-en-3d/"
published_at: null
first_seen_at: "2026-07-23T22:35:19.009797+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:0bc387009c4815538006ca06e10dfa8784df345c3e0d8d6306ffeee6f9f4f080"
---

# Orientation des pièces imprimées en 3D : Effets sur la résistance, la précision et l'état de surface

Il peut sembler évident de choisir l'orientation qui offre la hauteur en z minimale, et donc le temps de construction le plus court. Cependant, s'orienter uniquement selon la hauteur en z ne fait pas toujours sens en termes d'exigences pour votre pièce. Il y a quelques autres facteurs techniques à prendre en compte lorsqu'on pense à l'orientation des pièces pour l'impression 3D. Lorsque vous pensez à l'orientation des pièces pour l'impression 3D — spécifiquement la stéréolithographie (SLA) — pensez à un crayon. Que faudrait-il pour imprimer en 3D ce crayon couche par couche, soit horizontalement soit verticalement ? Le faire horizontalement prendrait moins de temps, car il est plus plat lorsqu'il est couché sur le côté, et l'imprimer verticalement prendrait beaucoup plus de temps, car il est debout et nécessite plus de couches dessinées empilées séquentiellement l'une sur l'autre. L'impression horizontale nécessiterait également plus de structures de support comparée à l'impression du crayon verticalement, et celles-ci nécessiteraient du temps supplémentaire pour être retirées afin d'obtenir une pièce finie. Ainsi, votre choix d'orientation pourrait dépendre des exigences de résistance ou des exigences de finition de surface pour votre crayon, et vous pourriez même vouloir orienter votre pièce à un angle. Il y a un ensemble de considérations lors de la sélection de l'orientation de construction en[stéréolithographie](https://www.protolabs.com/fr-fr/services/impression-3d/stereolithographie/) . Entrons dans le détail !


### Orientation des pièces imprimées en 3D pour la résistance


Regardons d'abord une illustration rapide qui explique les implications générales de l'accumulation de couches d'impression 3D et des forces externes sur les pièces. Lorsque les forces de tension sont normales, ou perpendiculaires, aux couches, une faiblesse de la pièce a tendance à se produire. Lorsque les forces de tension sont parallèles, ou alignées avec les couches, la pièce est plus forte. Lorsque les forces sont normales aux couches, la pièce est généralement capable d'accommoder plus de compression que si les forces sont alignées avec les couches. Deux conclusions s'appliquent :


1. Imprimez dans une orientation de sorte que les couches s'alignent avec l'axe où les forces de tension sont les plus élevées et
2. Imprimez dans une orientation de sorte que les couches intersectent l'axe où les forces de compression sont les plus élevées


Dans le cas théorique du crayon, une orientation de construction horizontale fonctionnera probablement pour tenir compte des forces de tension et de flexion que le crayon rencontrera lors de l'utilisation. Notez que ces directives s'appliquent toujours aux pièces en Modélisation par Dépôt de Fil Fondu, (FDM). Les pièces SLA présentent beaucoup moins d'anisotropie que les pièces FDM car les couches sont chimiquement liées, et la résolution obtenue avec la SLA est beaucoup plus élevée qu'avec la FDM. Cependant, les matériaux thermodurcissables peuvent nécessiter une réflexion supplémentaire autour de l'orientation des pièces pour maximiser la résistance des caractéristiques critiques de la pièce.


---


Un retrait différentiel est présent dans une pièce imprimée en 3D qui a été orientée comme indiqué ci-dessus. Le passage rapide d'une petite à une grande surface de section transversale a provoqué une rétraction sur cette pièce.


### Orienter pour éviter la rétraction différentielle


En général, plus la distribution des matières est inégale dans une pièce, plus celle-ci est susceptible d'être victime d'un phénomène connu sous le nom de rétrécissement différentiel. Certaines géométries et tailles de pièces sont intrinsèquement plus susceptibles de se rétracter parce qu'elles ne bénéficient pas d'un soutien adéquat de la part des couches de matière précédentes. Pour illustrer ce phénomène, prenons l'exemple d'une lettre majuscule "H". Lorsque la section de la barre transversale apparaît couche par couche au milieu, elle a tendance à tirer les pattes de soutien vers l'intérieur, ce qui fait que la pièce s'incline vers le bas. Pour éviter le rétrécissement différentiel, il existe plusieurs options. Souvent, nous construisons les pièces SLA sur un angle pour réduire la surface, et donc la contrainte globale rencontrée pour chaque couche. Cela permet également d'augmenter la dimensionnalité de la pièce et la précision du tolérancement. En outre, nous construisons des pièces en position couchée, de sorte que chaque couche formée a la même surface que la couche inférieure et qu'il n'y a pas de grandes variations de la section transversale. Lorsque des dimensions critiques ou des caractéristiques de pièces à haute résolution sont requises, nous conseillons d'orienter les pièces de manière à ce que ces caractéristiques soient positionnées sur la surface de la pièce tournée vers le haut. Gardez à l'esprit que la construction de pièces sur un angle peut donner des pièces avec une finition de surface non idéale si l'aspect cosmétique est essentiel.


Differential shrink can be avoided by orienting to eliminate small to large cross-sectional changes on the part (shown above) or by orienting the part at an angle (shown right).


If smooth, cosmetic features are the goal, build parts vertically. Building at an angle will cause visible layer lines as shown on the angled cylinder.


---


### Orientation pour obtenir l'état de surface souhaité


S'il est important que votre pièce ait une finition cosmétique pour une utilisation dans un salon ou une exposition, nous aurons tendance à construire la pièce à plat et non à l'oblique. Toutes les caractéristiques critiques doivent être orientées vers le haut, car les surfaces supérieures seront lisses et plus enclines à la précision dimensionnelle. En revanche, si votre pièce est construite vers le bas, les structures de soutien qui doivent être retirées et grenaillées élimineront tout effet de brillance.


The glossy effect is shown on the “H” that was built lying flat. The “H” built at an angle highlights layer lines and the visual effects they can have on text.


En règle générale, notre équipe peut choisir une orientation différente pour vous fournir une pièce de la meilleure qualité. Il est important de garder à l'esprit que si vous ne précisez pas l'orientation souhaitée de la pièce (horizontale ou verticale), nous choisirons l'orientation qui nous semble être le meilleur compromis entre le temps de fabrication le plus rapide possible et une bonne optique. En nous indiquant les aspects les plus importants pour vos pièces imprimées en 3D, vous nous aidez à déterminer la meilleure façon d'orienter votre pièce au cours du processus de fabrication. Dans l'ensemble, ce qu'il faut retenir, c'est que si vous avez un besoin particulier pour une pièce imprimée en 3D, qu'il s'agisse de caractéristiques critiques, de considérations de coût ou d'une finition de surface spécifique souhaitée, assurez-vous d'en informer notre équipe d'ingénieurs à l'avance afin que nous puissions nous assurer que votre pièce réponde à vos objectifs spécifiques.
