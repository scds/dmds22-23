# Lesson: Basic text prep with Python
by Alex Provo and Jay Brodeur for [ARL DSI 2021](https://jasonbrodeur.github.io/dsi-text-prep/)

In this notebook, we'll run through a few examples that introduce you to text exploration, manipulation, and transformation using Python. The purpose of these exercises is to familiarize you with basic concepts of preparing text in Python, and hopefully, encourage you to explore it a bit more. 
  
Throughout this notebook, we've tried to document as much as possible the operations that we're performing, and the motivations behind doing so. More than anything, we're trying to convey the exploratory and often iterative nature of developing scripts and functions to prepare text for further analysis.  
  
Towards the end, we'll demonstrate some of the interesting things you can do with Natural Language Processing (NLP) packages like [NLTK](https://www.nltk.org/) and [spaCy](https://spacy.io/). There are **many** more tools and resources to explore, and we've listed some of them in the [Learn More](https://jasonbrodeur.github.io/dsi-text-prep/learn-more.html) page of the workshop website. 

We hope you enjoy!

In this exercise, we will continue working with the article by Bernhard Berenson that was used in the [Open Refine](https://jasonbrodeur.github.io/dsi-text-prep/open-refine.html) exercises. 

**Citation:** Berenson, B. (1892). Some Comments on Correggio in Connection with His Pictures in Dresden. The Knight Errant, 1(3), 73-85. doi:10.2307/25515893
  
Depending on our overall objective with the text (e.g. advanced analysis, dissemination of "clean" text, our objectives may include: 
* Identify and remove or fix errors--ideally, using repeatable methods that can be used on this text and perhaps many others in the corpus
* Reduce the words in the text that have little value (very common 'stop words' like 'a', 'an, 'me', 'my', 'myself', 'we', 'our', 'ours', etc.) to improve the information content for analyses. 
* Perform the desired analyses and/or disseminate the "cleaned up" text. ("cleaned up" is placed within scare quotes here to acknowledge the subjectivity involved in making a value judgment about what is "clean" and what should be "cleaned")

In this exercise, we're going to perform the following tasks: 
1. Load the data and inspect it for issues that may impact our later work
1. Tokenize the text into individual words
1. Identify patterns within these issues that we can use to build repeatable processes to address them
1. Use patterns and regular expressions to remove recurring errors
1. Experiment with other modules (such as a spell checker) to assess their utility in preparing our text.
1. Remove punctuation and convert text to lowercase
1. Remove stop words
1. With the NLTK package, begin building a repeatable 'pipeline' for preparing our text
1. Explore other advanced packages like spaCy and perform Named Entity Recognition analysis.

## 1. Load and inspect
First, let's load the file and preview the contents of our file. 


```python
# Read the text file
with open('careggio-raw.txt') as f:
    text = f.read()
    
# Print the text to the screen
print(text)
```

     i _ IPS^S^ffclS OME comments on correg ncRftJH GIO IN CONNECTION with 1^58^^^ HIS PICTURES IN DRESDEN. Spl^^T^ES A few years ago, it would have been hard u|^J^^fev^S to tell whether Correggio's Night or E^^g^^M Raphael's Madonna Di San Sisto was the ^L^^^^^M favourite picture of the Dresden Gallery. mmSSSmS^mSSm The little sanctuary where the Virgin with Saint Sixtus floats above the pseudo-altar was then crowded with worshippers as it is now, and Correggio's picture had quite as large and devout a fol lowing. But some change in popular taste has evidently taken place, for few people now linger before the Night. What inference is to be drawn ? Was the enthusiasm for Correggio merely a fashion which has had its season ? He is certainly no longer admired as he was in the first few decades of this century, in the day when no gentleman could afford to be without his theory of the " Correggiosity of Correggio." The explanation is not far to seek. The enthusiasm for Correggio dates from the time when, all the possible variations having been played upon the themes introduced by Raphael and Michelangelo, the Caracci betook themselves to a comparatively unlaboured field, and founded upon Correggio their school of painting, and thus succeeded in lending a new life to Italian art. Most people, however, appreciate only what is of their own day, ana Correggio's in terpreters proved far more interesting to their contemporaries than the master himself. The Caracci, Domenichino, Guer cino, Guido Reni, and Lanfranco used up all the aesthetic capacity of their admirers, who believed in Correggio as the Catholic peasant doubtless believes in God, although he makes his offerings to the Saints. Furthermore, it was by no means easy to know the master himself. Correggio lived to be scarcely forty. Of his pictures then known the earliest dates from his twenty-first year, and in a career of barely twenty years no painter could have painted enough to fill the various collections of Europe. But in the third decade of this century, the few whose word was law in matters of taste sud denly turned away from Guido, Lanfranco, and their like, and gave themselves up to an unbridled enthusiasm for the Caracci and for their master, Correggio. Later, even the Caracci dropped out of sight, and Correggio stood alone. The Madonna with St. Francis, No. 150. The Madonna with St. Sebastian, No. 151. The Nativity, called the " Night," No. 152. The Madonna with St. George, No. 153. 7373
    
     The change was due in part to the fact that Correggio at last found in Toschi, the engraver, a perfectly accurate trans lator and publisher. If engraving is considered as a fine art by itself, there have been many greater masters of the craft than Toschi; but no one ever assimilated more thoroughly than he the style of a great painter of several centuries before, or ever gave such faithful, such impersonal renderings from an old master. When his engravings had made it really possible to know Correggio, the public placed him at once in the highest heaven. Nor, in this instance, were they wrong. The Correggio with whom they thus made acquaintance, the painter of the frescoes in the Convent of St. Paul at Parma,? frescoes filled with delightful Cupids playing hide-and-seek in garlands of flowers,? had a genius quite as fine as any artist of his time. Toschi's garlands and Cupids must have been tantalizing to the lovers of Correggio, but the originals were far away. Fortunately there were several Correggios in Dresden, which was near at hand, especially to the various seats of aesthetics, such as Jena, Weimar, and Gottingen. In one of the Dresden pictures both the Cupids and the garlands were to be seen, and this picture, the so-called St. George, became at once a favourite, and of course a masterpiece. There is another reason, perhaps even more cogent, for the sudden popularity of the St. George. A change in taste is no more completed in a day than a change in character. The St. George is one of Cor reggio's latest paintings, and, as followers always take up a master's methods where he left them, this picture was one of those upon which the Caracci formed their own style. People were well acquainted with the Caracci: so they found it easy to appreciate a work in which Correggio differs but little from them. The St. George was painted about 1532, two years before Correggio's death. It represents the Madonna seated upon a throne of very barroque design, surrounded by St. George and other Saints. Her face is flabby and puffy, though not with out a certain charm, and her eyes are very large. Her knees and her breast come close together: you can almost see the soles of her feet. To understand her appearance you must imagine yourself looking up at her from the bottom of a well. When Correggio painted this picture, he had already finished his frescoes in the dome of the Cathedral of Parma. These are so full of movement, contain such startling feats of foreshortening, that the painter of them seems to have fallen a victim to the admiration of his own cleverness. At any rate, he never afterwards drew a figure in repose, or in a 7474
    
     s normal position. He seemed to have used up his natural vein of feeling, and having used it up, his interest narrowed itself down to making his compositions animated and grandiose, debasing the human figure to purposes as vile as the con torted atlantides of barroque architecture. When this happens, when a painter begins to think of experimenting with the lines of the human figure as something merely " composable," instead of regarding them as a means of expression; or, worse still, when he is possessed by the desire to exhibit his clever ness, there is little further to be said about him as an artist. If the St. George were well preserved, which is far from being the case, one might say, in the language of the Parisian ateliers, " Cest trte chic, ca.' Chic, hollow cleverness, is all that the picture can be said to have; it must be added, however, that it has this to a degree that makes it really enjoyable. But the picture had a further charm which appealed to the connoisseurs of the day. To understand it, we need an idea of the dominant spirit of German literature during the twen ties. Its principal exponents were Novalis, Fouqu6, Tieck, and the Scnlegels. Goethe's reign had passed, Heine's had not yet begun. It was the decade of pure Romanticism, which, in Germany, was by no means that innocent, rough and-tumble movement of liberation from prim manners and bad couplets it was in England. The heroine of Miss Austen's " Northanger Abbey" is a much better representa \ tive of English romanticism than the somewhat hectic Mari anne of "Sense and Sensibility." But even Marianne would have seemed a very coarse creature to the cultivators of the " Blue Flower." (German romanticism carried sensibility to the point where it becomes insanity. Health was thought vulgar. It was the fashion to seem diaphanous, to cough tellingly, to look worlds, and to take the greatest interest in what was of no human concern. The St. George in Correg fio's picture was not diaphanous or consumptive, to be sure, ut he certainly had the morphine habit, which would make him quite as interesting. His foot rests upon a dragon, no doubt a symbol of his victory over the demon of Morphia. The children, with their huge heads and watery eyes, are monsters that might have been suggested by some fantastic tale of Hoffman's. St. Peter Martyr talks theology with the sincerity of August von Schlegel, and St. John is sufficiently like Antinous preparing for a ballet to have rejoiced Wilhelm von Schlegel, the Gottingen professor of aesthetics. The only figure in the picture that has any health in him is St. Gemig niano, and he hides in the background as if ashamed of being 7575
    
     so robust. For these reasons, then, the St. George became the standard, the canon of Correggio, and his other pictures were judged accordingly. The Night, being nearest to it, came next in public favour. Indeed, when Romanticism began to go out of fashion, it became the supreme favourite. This altar-piece was painted at least two years before the St. George. The subject is the Nativity?a subject so of ten Eaintea that Correggio might well have asked himself how e was to avoid the commonplace in treating it once more. I am inclined to think that the painter tried to interpret the divine event from a point of view as human and lowly as that of the Gospels themselves. The Madonna is in the first place a young mother, the Child is a mere human infant, and the Shepherds are nothing but shepherds. St. Joseph, instead of being the usual supernumerary, is occupied in leading away a mule, who lingers, attracted by the light, or perhaps oy the straw. There is no conventional choir of angels. His angels are too wild with joy to pose languidly with mandolins in their hands. It was the sneer humanity of this picture that drew so many pilgrims to it, and not, as the critics of that time said, because Correggio had the wonderful idea of mak ing all the light stream from the Infant's face. Correggio may have had some such purpose, only as an intention it is rather literary than pictorial, and it is more likely that he had something in mind far less theological and poetical. His idea seems to have been to experiment with lights. From the Child's face the light streams out into the darkness, and dies away just before it encounters the first white of dawn appear ing over the horizon. In the present condition of the picture, it is no longer possible to judge what was the effect. That it must have been very wonaerlul there can be no doubt. But even if the effect of the meeting of half lights and reflected lights at a point darker than either could still be appreciated, it would remain true that not the lights, but the human inter pretation of the subject, made the Night so great a favourite. A real feeling for artistic treatment as distinct from illustra tion must now be much more wide-spread than it used to be, otherwise it is hard to explain why the Night has so fallen from grace. If the appreciation of painting has increased to such a de gree that it is no longer possible to be very enthusiastic about these two pictures, once so much admired, does it mean that there is nothing left to enjoy in Correggio ? As the knowledge of the old masters advances, and as, with the aid of discriminating criticism, the power to enjoy them 7676
    
     increases, it is found more and more that the latest works of a painter are not necessarily his best. Indeed it seems that many of the Italian masters were most fascinating soon after they began to paint, or at any rate, while they were still young. This was especially true of painters such as Correggio who were more sensitive, perhaps, than vigorous?lyric rather than epic. The Dresden gallery possesses a picture by Correggio which the leaders of aesthetics in the early decades of this century scarcely deigned to notice. It was done in 1514, in his twenty first year, and being so early a work, it deserves careful atten tion. It represents the Madonna with the Child in her lap, enthroned under an arch, with four Saints at her feet, St. Catherine and John the Baptist on her left, and St. Francis and St. Anthony of Padua to the right. Above, just under I the arch, two little angels are poised in the air, as if it were i water in which they floated joyfully at their ease. Between them is a halo, or glory, with ruddy cherubs peeping out from f| the straw-coloured light. The little angels are restful, the Child is quiet and simple, as different as possible from the ff nervous imp in the St. George. The Madonna has a face in which there is nothing mystifying, nothing theatrical. The Eoses of the Saints are dramatic, their interest is very quick; ut they are not at all so melo-dramatic as even Raphael's St. ?51 I?*111 *n the Madonna Degli Ansidei, in the National Gallery. %';'; ; The scheme of colour is bright and clear, but quiet. The arch behind the throne opens upon an unobtrusive landscape. Although this is not among the severest nor yet among the rl most majestic altar-pieces ever painted, it is one of the most - |; delicate and most felt. Modern criticism, then, takes this II early work as a starting point for the study of Correggio. A W few years ago it was supposed, indeed, to be his earliest paint W-\ ing; but to-day we know nine or ten which he must have done i before. But as it was the study given to the St. Francis which led to the discovery of the earlier paintings, we can do nothing better than stop here and see what this picture can reveal of Correggio's history. You cannot look long at the picture without the haunting feeling of having seen elsewhere something very much like it. The truth is that the St. Catherine here bears a strong resem blance to Raphael's St. Catherine in the National Gallery, and the St. John is not unlike the St. John in his Madonna Di Foligno, of the Vatican. Other resemblances might be traced; but these are enough. The phenomena of art are as certainly governed by laws as 7777
    
     the phenomena of nature. The province of art criticism is to discover these laws, which prove that in art, as in nature, there is no such thing as mere coincidence. Striking resemblances such as these must be accounted for. It is of course out of the question that Correggio was the pupil of Raphael; but recent researches have given us the clue to the real connection between them. It is now admitted that Raphael owes much of what is characteristic in his style to nis first master, Timoteo Viti, and, through him, to Francesco Francia and Lorenzo Costa, Timoteo's teachers at Bologna. Correggio's training has been traced to the same sources, for he also was, directly or indirectly, the pupil of Costa and. Francia. On the wall opposite to the St. Francis hangs a characteristic picture by Francia? The Baptism of Christ. The moment your attention is called to it, you perceive that the movement of the figures in both pictures is strikingly the same; that the eyes in both are opened wide in the same way, and that the general scheme of colour is not unlike, the reds and yellows, indeed, being identical. Correggio's is finer subtler, more delicate, but they differ only as members of the same family. The influence of Francia, however, does not explain every thing in Correggio's picture. The Madonna's face, for in stance, suggests Costa s type, rather than Francia's, and the figure of Moses in the medallion on the throne is taken almost direct from Costa. The medallion itself, the elaborate throne, the bas-reliefs on its base, are all of them peculiar to Costa and to the Ferrarese school from which he came; and their presence in a picture by Correggio is sufficient ground for placing him among the painters of the school of Ferrara. But Correggio used to be described as a Lombard painter whose first master was a certain Bianchi, but who owed his real training to Mantegna. Mantegna, however, died in 1506, when Correggio was scarcely twelve years old. At that age, precocious even as he was, he could scarcely have done more than acquire the rudiments of his craft, and it is not likely that instruction received so young would have finally deter mined his style. Nor, indeed, is there much in the St. Francis to indicate a personal relation between Correggio and Man tegna. The pose of the Madonna, to be sure, is taken from Mantegna's Vierge Des Victoires now in the Louvre, which Cor reggio could have seen nowhere else than at Mantua. Poses, however, or even whole episodes, borrowed from another painter, no more prove direct descent than, for instance, the Latin words we nave borrowed prove that English was de rived from Latin. Although there is no ground, therefore, for 7878
    
     the belief that Correggio was a pupil of Mantegna, the St. Francis confirms the old theory about Bianchi. The young painter was taught, just as children are taught writing, to draw the human form after a set fashion, especially such parts as the hands and ears, which are very obvious, and yet, when looked at carelessly, without much individuality. Naturally the method was the master's own, and the pupil kept it all his life, in spite of gradual and great variation. This fixed manner of drawing the hands and ears often assists us greatly in seeing the connection between a painter, his master, and his pupils, or in determining the authorship of a picture. You cannot look at the hands in the St. Francis with out noticing not only that they are well modelled, that they are refined, but that the second finger of each is too long. Even if this were a personal idiosyncrasy, it is not an accident, for it is found in most of Correggio's pictures. Bianchi, whom the legend mentions as Correggio's teacher, is now being re discovered after having been almost forgotten for centuries. His masterpiece, whicn shows that he was a painter of the Ferrarese school, is a {Madonna and Saints in the "Louvre. Few pictures even of that wonderful collection can surpass it for grandeur of composition, subtlety of feeling, clearness of colour, and quietness of tone. The flesh colour is very white. Make it less smooth, model it a little more, and you have the unrivalled flesh of the tAntiope, Correggio's greatest achieve^ ment in flesh painting. The hands in this picture by Bianchi are not only shaped somewhat like the hanas in the St. Francis, but they have also the same characteristic, the elongated second finger. This goes a great way to prove that the legend was right in calling Bianchi Correggio's first master. Another thing that a pupil learned to do after a set fashion was the landscape. In the St. Francis the landscape is in tone and colour, if not altogether in drawing, identical with that in Bianchi's Madonna. Indeed, Correggio, allowing for the ad vances he made of himself in the differentiation of light, and in aerial perspective, remains always true to this type. In tone and colour the landscape even of the St. George, painted, it will be remembered, two years before his death, is much like Bianchi's. Correggio probably left Bianchi and went to Francia and Costa at Bologna in 1508, when he was fourteen years old. In 1509 Costa went to Mantua to take the place left vacant by Mantegna, and there is good reason to believe that Cor reggio went with him and remained there for several years. We have already seen that the St. Francis proves his presence 7979
    
     at Mantua before 1514, and a still earlier picture, belonging to Signor Crespi of Milan confirms the proof; for two of its figures ?St. Elizabeth and the Infant John ?are taken di rectly from Mantegna's picture which still hangs in his mort uary chapel at Mantua. Correggio's stay in Mantua brought him in contact with a painter under whose inspiration nis work took on a character which was altogether more modern than Costa's. This painter was Dosso Dossi, who was in Mantua in 1511 and 1512. His influence can be traced in the whole series of Correggio's early pictures which ends with the St. Francis. In this altar-piece the peculiar colour of the halo, like pale sulphur, and the ruddy cherubs which frame it in, are strikingly like the halo and the cherubs in a small Corona tion of the Virgin by Dosso, which is also in the Dresden gallery. The little angels below the globe in Dosso's picture are posed in a way which suggests at once the pose of the Christ Child in the St. Francis, ana in nearly all the early Correggios?in the Holy Family of Hampton Court, in the Madonna of the Muni cipal Museum of Milan, in the Madonna and Saints belonging to Signor Frizzoni, also in Milan, in the Madonna at Pavia, and in the (Madonna with Angels of the Uffizi. It would not be hard to weave a romance about Correggio's relation to Dosso. In 1511 Dosso Dossi was thirty-two years old, and nearly at the height of his genius. It was just before he became the court painter of Alfonzo of Ferrara and Lucre tia Borgia, his wife. Ariosto speaks of him in his Orlando along with Giorgione and Leornardo and Michelangelo. The court poet and the court painter were remarkably alike in the essence of their genius. They were both lovers of " high romance," and both had the power to create it?the one in verse, the other in colour?with a splendour that per haps many other Italians could have equalled, but with a fantasy, a touch of magic, that was more characteristic of English genius in the Elizabethan period than of Italian genius at any time. Real feeling for the fantastic and magi cal was not often granted to the well-balanced Italian mind. It is all the more delightful, therefore, to find an artist who has not only the strength and self-possession of an Italian, but the romance and sense of mystery of the great English poets. If Marlowe had written about Circe, he would have presented us with one like Dosso's, as she may be seen in the Borghese gallery: an enchantress clothed in crimson and emerald, sitting under balsamic trees where olive green lights are playing, with the monsters about her feet, their real na tures made visible by her arts. Rather than Greek she is 8080
    
     Arabian. She gives us no time to ask whether the lines of her form are classical, or whether her form is statuesque. Before her we lose ourselves in a maze of strange lights and mysterious colours which make us sink deeper and deeper into a world which is as entrancing as it is far away. Mar : lowe and Shakespeare would have taken that delight in her which we can well imagine Ariosto took. The painter of pictures like this could not help having an extraordinary fascination over such a sensitive, dreamy, / ecstatic temperament as Correggio's. It is easy to imagine : the precocious lad of sixteen, with his training already far advanced, but with faculties interpretive rather than creative, \ falling down in worship before the dazzling achievements of the Ferrarese painter. Dosso's personal charm, also, must have been great, and he was just at the point in his life when the man is the boy's ideal. Fellow artists, then as now, we | may be sure, talked of nothing so much as of their craft. k Dosso, who had been in contact with the Venetians, and with pupils of Raphael, was in touch with many of the problems ^ that interested the painters of the day. Nothing occupied % the best painters just then so much as the problem of light ? the eternal problem of painting. In Florence, Pier dei ?t Franceschi, Verrocchio, and Leonardo; and in Venice, Alvise r Vivarini, Carpaccio and Giorgione had brought the treat-. |V ment of light to a point far beyond anything Costa could | have known. It was just this advanced treatment which was I necessary to give Correggio the means to develop his peculiar f talent; for he was not aestined to create new types or new Va subjects. It was his destiny rather to be among the first to I treat his subject for the personal feeling and not for the mere v action ? still less for the mere composition. When he is at ; his best, he not only makes the face but the whole figure, and the landscape as well, the vehicle of emotion, and this to such \" a degree that to go beyond is to become a Guido Reni. But he - never could have accomplished this with the limited acquaint ance with light his first masters gave him. From Dosso he : got the impulse for that study of the effects of light, which r itself became in his hands a means of expression utterly un dreamt of heretofore. It is easy to trace this connection between them. In almost every picture of Dosso's, where the subject and composition permit (as, for instance, in the small Coronation already men tioned), the groups are so arranged that in looking at the land scape, one seems to be looking out upon it from within a cavern. This is the case to an even greater degree in a larger 81 *81
    
     Coronation by Dosso, which hangs opposite to the St. Francis. Where this cavern-like effect is not possible, Dosso used to light one part of his picture much more than the rest, as one may see in his St. George, which hangs above the St. Francis. In short, his pictures never present the appearance of the greater part of the paintings done before his day,? the appearance of an infinitesimally low relief. On the contrary, he batters great holes into his compositions, huge pockets, as it were, and fills them with light. To make the contrast even greater, he gives a slate-gray colour to the rim of this well of light, if not to the whole darker portion of the picture. Correggio took this treatment from Dosso, refined and advanced upon it, but Dosso's treatment of light and shadow contains in embryo all Correggio's. The first instance of this in Correggio occurs in a picture already mentioned, belonging to Signor Crespi of Milan. It is a Nativity. The Holy Child is lying in a wicker basket on the ground. The Virgin kneels beside Him. Two little angels are floating above, like the angels in the St. Francis. To the left, from the direction where the light is breaking, two other angels lead up the pious shepherds. Not only does the light in this picture all stream from one corner, but the general tone is slate-gray. Looked at merely as light and tone it is identical with Dosso's St. George. It is easy to see the same influence in many other details, as, for instance, in the drawing of the eyes and mouths, which Dosso, in his works, was inclined to make like black holes. This peculiar way of making the eyes and mouths, transmitted to Correggio, is even more clearly visible in a picture belonging to Mr. Robert Benson of London. It must have been painted somewhat later than the Nativity. The subject is Christ taking leave of His Mother. Clothed in white, He kneels with crossed arms at His mother's feet. She seems to be on the point of rushing for ward to embrace Him, but is held back by Mary. The Virgin's face expresses the greatest grief, but nothing wild or un seemly. St. John stands a trifle back, with his hands clasped in sympathizing sorrow. The distribution of light here is even more Dossesque than in the Nativity. It all comes from the left hand corner, whence it breaks over the Lake of Gali lee, broods over its surface with a pale gray light, flashes up to the sky in a greenish streamer, and is reflected on Christ's raiment, and on His mother's face. So we might take up Correggio's earliest paintings, one after another, and find Dosso in them all. I must mention one more at all events. It is an Epiphany, in the Archbishop's 8282
    
     i?i"" ' * ? palace in Milan. At first glance, it is hardly to be distin ffuished from a Dosso; but a more careful examination leaves ittle doubt that it is by the younger painter, who, in this in stance, seems to have caught, along with Dosso's way of painting, something also of his feelW for the romantic. Dosso's influence comes out again in the T{est in the Flight, Eainted a little before the St. Francis. Of the three pictures y Correggio that the Uffizi possesses, this is by far the most beautiful. The deep-set eyes, the pale sulphur colour in the Madonna's robe, the dark wood, are all reminders of Dosso, and the Virgin herself might be half-sister to the Circe. Having deducted the story of Correggio's growth bit by bit, it will perhaps not be found amiss to sum it up briefly. Cor reggio got his rudiments from Bianchi, who handed him on to Francia and Costa. Costa took him to Mantua, where the works of Mantegna seem to have made only a passing impres sion on him. There he came in close contact with Dosso Dossi, who helped him to acquire a method of painting which gave full scope to his genius. But to complete the account of the influences which went to form him, it remains for me to speak of a picture in Munich?the little Faun?so strikingly Venetian in character that Dosso's influence alone will not ex plain it. In colour it is like a Palma, in movement it is like a Lotto. It leads inevitably to the inference that Correggio must have visited Venice before finally settling down near Parma ? an inference that might explain the puzzling like nesses between Lotto and Correggio that keep forcing them selves upon our attention. Art is a flower of the human personality. Flower-like, it breathes out perhaps not its strongest, but often its most deli cate, perfume soon after bursting. It is delicious to catch an artist's naive impressions of the world, and this is one of the rewards of studying the earliest works of a painter. In the Italian Renaissance, at least, if a man was born with some thing to say in form and colour, he was likely to say the best of it very soon after he had fair mastery of his brush, rather than later, when manifold commissions, family concerns, and the ever advancing invasion of the commonplace, made him think of his painting less as an art and more as a business. This is above all the case with Correggio, whose genius was so distinctively subjective and lyrical. The pictures al ready discussed bring out this point clearly. The briefest comparison, for instance, between Signor Crespi's [h{ativitv, painted when Correggio was sixteen or seventeen, and the same subject as treated in the Slight, when he was thirty-five, 83 4?83
    
     shows that although he had made immense technical ad vances in the later picture, he had lost that intense and poeti cal religious feeling which made the early picture so impres sive. Or, again, the Rest in the Flight of the Uffizi has a per sonal quality which somehow the later picture of the same subject at Parma, beautiful as it is, utterly lacks. In the first, you feel that Correggio tried to live the scene before painting it; in the other, that he is reciting it like a lesson well learned. Even in the St Francis, he had lost something of the religious imagination he had when he was at work on the picture of Christ taking leave of His Mother. There he had an intensity of feeling and a reserve of expression which we no longer find in the later picture, where the feeling is much more ordinary and the expression at the same time a little exaggerated. Another advantage of studying a painter from his begin nings arises from the fact that there is a large intellectual ele ment even in pleasure supposed to be purely aesthetic. A Eainter naturally shows more clearly at nrst than afterwards ow he is connected with the other painters of his time. We get to know his first forms, to see how they have come down to him from his teachers, and, finally, how they are trans mitted by him to his pupils. If we learn to like his charac teristics tor their beauty and for their historical associations, we continue to like them, no matter how disguised. The ability to trace them in a picture makes that picture to some degree delightful in spite of the faults it may otherwise have. It is, for example, a pleasure to find in the St. George that the landscape and the hands still retain something of their early likeness to Bianchi. Although there is little left to enjoy in the Night, yet an acquaintance with Signor Crespi's Nativity makes it most interesting to see how the painter treated the subject after a lapse of twenty years. So that the study of the early works of a master not only reveals him at a period when he is likely to be very charming, but makes him inter esting even in his decadence. It has not been my purpose to speak of the works of Cor reggio's maturity. In the period between the St. Francis and the St. George, he painted such pictures as the Antiope and the Leda, the Danae, the lo, and the Ganymede, pictures as intensely lyric as his earliest are sincerely religious. It only remains to mention another picture in the Dresden Gallery dating from this same period. It is the so-called St. Sebastian ? the Madonna on a throne with cherubs and angels about her, looking down upon St. Sebastian and two other saints. It is. a picture with the same movement and the same feeling that 8484
    
     we find in the Leda, the Io, or the Danae, even the face of the Madonna being very much like what the Leda must have t\: been. Like all these masterpieces it is full of that high-strung f-> sensuous emotion which inevitably suggests the music of violins. But of course such movement and such feeling are \' utterly out of harmony with the subject of a religious picture, where the effect to be produced is one of awe and devotion, not of fellowship with the gods in ecstatic enjoyment. Dresden, October, 1890. Bernhard Berenson. Florence, February, 1892.85
    
    
    


Take a few minutes to inspect the text. What do you notice? 
* Are there obvious OCR errors present in the text? What might have caused these issues? 
* Are there other, less egregious spelling errors? 
* Are there patterns or similarities to the errors? 
* Is there extraneous information included in the text (e.g. page numbers, footnotes, etc.)?

## 2. Tokenization
Let's explore a few ways that we might address these issues. Our first step is to tokenize the text--which is to split the text into individual words that we can use in further preprocessing and analyses. 

Here, we will split the text by blank space (a decent place to begin with English text) 


```python
# Split into words by whitespace for a quick inspection:
words = text.split()
orig_num_words = len(words) # count the original number of words in the text.
# Print a statement the communicates the total number of tokens:
print("number of words: " + str(len(words)))
# Print the first 100 words for inspection: 
print(words[:100])
```

    number of words: 5776
    ['i', '_', 'IPS^S^ffclS', 'OME', 'comments', 'on', 'correg', 'ncRftJH', 'GIO', 'IN', 'CONNECTION', 'with', '1^58^^^', 'HIS', 'PICTURES', 'IN', 'DRESDEN.', 'Spl^^T^ES', 'A', 'few', 'years', 'ago,', 'it', 'would', 'have', 'been', 'hard', 'u|^J^^fev^S', 'to', 'tell', 'whether', "Correggio's", 'Night', 'or', 'E^^g^^M', "Raphael's", 'Madonna', 'Di', 'San', 'Sisto', 'was', 'the', '^L^^^^^M', 'favourite', 'picture', 'of', 'the', 'Dresden', 'Gallery.', 'mmSSSmS^mSSm', 'The', 'little', 'sanctuary', 'where', 'the', 'Virgin', 'with', 'Saint', 'Sixtus', 'floats', 'above', 'the', 'pseudo-altar', 'was', 'then', 'crowded', 'with', 'worshippers', 'as', 'it', 'is', 'now,', 'and', "Correggio's", 'picture', 'had', 'quite', 'as', 'large', 'and', 'devout', 'a', 'fol', 'lowing.', 'But', 'some', 'change', 'in', 'popular', 'taste', 'has', 'evidently', 'taken', 'place,', 'for', 'few', 'people', 'now', 'linger', 'before']


## 3. Removing 'garbage' text
One type of error that stands out are the 'garbage' words that contain multiple carets ```'^'```.  
Let's identify all words that contain a caret to see if we can remove all of them to improve our text


```python
caret_words = list() # make a blank list that will hold our caret words.

# Iterate through the entire list of words. If a '^' exists, add it to our 'caret_words' list.
for i in words:
    if '^' in i:
        caret_words.append(i)   # Add to the list of caretted words
# Print the list: 
print("Caretted words : " + "\n" + str(list(caret_words)))
```

    Caretted words : 
    ['IPS^S^ffclS', '1^58^^^', 'Spl^^T^ES', 'u|^J^^fev^S', 'E^^g^^M', '^L^^^^^M', 'mmSSSmS^mSSm', 'achieve^', '^']


Reviewing the list, it seems as though making a rule that removes all words with a caret will mostly work, but we're going to end up also removing ```achieve^```, as well, which we would rather not do. So, we may want to try to find a rule that distinguishes the 'real' word from the 'garbage' ones.

Most of the other garbage strings have either a capital letter or numeral in them. We can use a regular expression to find all strings that have a caret AND either a capital letter or numeral.

To do this, we'll use [regular expressions](https://docs.python.org/3/howto/regex.html). There's a built-in module in Python called ```re``` that we can import to do this.


```python
# import the regular expression module
import re 

# Iterate through the entire list of words. If a '^' exists in a word along with a capital letter 
# or a numeral, add it to our 'caret_words' list.
caret_words = list() # make a blank list that will hold our caret words.

for i in words:
    a = re.search('[A-Z0-9]',i) # regular expression search for any capital letter or numeral
    b = re.search('[\^]',i)     # regular expression to search for a caret ('^') symbol
    if a and b:                 # An if statement that is true if both regular expressions find a match
        caret_words.append(i)   # if true, add to the list of caretted words to be removed
        words.remove(i)         # if true, remove the matching words from our list
        
# Print out a list of the offending words:
print("Caretted + capital/numeral words: " +"\n" + str(list(caret_words)))
print("Original number of words: " + str(orig_num_words) + "; Number remaining: " + str(len(words)))

# Print the first 50 words to inspect: 
print("\n" + "First 50 words: " + "\n" + str(words[:50]))
```

    Caretted + capital/numeral words: 
    ['IPS^S^ffclS', '1^58^^^', 'Spl^^T^ES', 'u|^J^^fev^S', 'E^^g^^M', '^L^^^^^M', 'mmSSSmS^mSSm']
    Original number of words: 5776; Number remaining: 5769
    
    First 50 words: 
    ['i', '_', 'OME', 'comments', 'on', 'correg', 'ncRftJH', 'GIO', 'IN', 'CONNECTION', 'with', 'HIS', 'PICTURES', 'IN', 'DRESDEN.', 'A', 'few', 'years', 'ago,', 'it', 'would', 'have', 'been', 'hard', 'to', 'tell', 'whether', "Correggio's", 'Night', 'or', "Raphael's", 'Madonna', 'Di', 'San', 'Sisto', 'was', 'the', 'favourite', 'picture', 'of', 'the', 'Dresden', 'Gallery.', 'The', 'little', 'sanctuary', 'where', 'the', 'Virgin', 'with']


## 4. Using a spellchecker
As mentioned in the instroduction, there are A LOT of python packages available for various NLP operations. For this example, let's use a relatively simple one called ```pyspellcheck``` . This package has been pre-installed for you in this Notebook. If you were using a regular Python installation, you could install it with the command: ```pip install pyspellchecker```. Note that the [pyspellcheck website](https://pyspellchecker.readthedocs.io/)--as is the case for most packages--has excellent documentation about how to use it. 

In the example below, let's see if we can use it to help us identify misspelled words in our text



```python
# import the SpellChecker module from the pyspellchecker package
from spellchecker import SpellChecker

spell = SpellChecker() # initialize SpellChecker as an object

# run SpellChecker on the first 100 items in our list of words and return only misspelled
misspelled = spell.unknown(words[:100])

# print the list of mispelled items from the first 100 words:
print("misspelled words: " + str(list(misspelled)))

```

    misspelled words: ['sisto', 'ncrftjh', 'lowing.', 'place,', 'sixtus', 'dresden.', "correggio's", 'gallery.', 'ago,', 'now,', 'night.', 'pseudo-altar', 'correg']


What do you notice about the list? It caught some obvious mispellings (like correg, ncrftjh), but has a number of false positives (like 'night.' ,'gallery.', 'ago,')?

## 5. Removing punctuation | Converting to lowercase
Clearly, we need to get rid of the punctuation before we do any more processing, as it is causing correctly-spelled words to be falsely identified. This is an excellent illustration of the ways in which programs can be very literal and inflexible. It also underscores the importance of operations like punctuation removal before persuing higher-order NLP processes.  

It's also a good time to normalize our case by reducing everything to lowercase. You might have noticed that the spellchecker is doing this for us before checking the spelling. Doing this may not be appropriate for certain approaches (like identifying proper nouns), but it will be necessary if we want to use the list of misspelled words to correct or remove errors from our text. 

To filter out punctuation, let's use the built-in list of punctuation from the 'string' package.


```python
# Use the built-in list of punctuation from the 'string package'
import string
# Print the list of punctuation to the screen
print(string.punctuation)

# First, let's merge our list of words back into a single block of text: 
text_rejoined = " ".join(words)

# Remove punctuation with a regular expression: 
words = re.split(r'\W+', text_rejoined)

# Print the first 50 words to inspect: 
print("\n" + "First 50 words: " + "\n" + str(words[:50]))

# Print an update on number of words remaining
print("Original number of words: " + str(orig_num_words) + "; Number remaining: " + str(len(words)))
```

    !"#$%&'()*+,-./:;<=>?@[\]^_`{|}~
    
    First 50 words: 
    ['i', '_', 'OME', 'comments', 'on', 'correg', 'ncRftJH', 'GIO', 'IN', 'CONNECTION', 'with', 'HIS', 'PICTURES', 'IN', 'DRESDEN', 'A', 'few', 'years', 'ago', 'it', 'would', 'have', 'been', 'hard', 'to', 'tell', 'whether', 'Correggio', 's', 'Night', 'or', 'Raphael', 's', 'Madonna', 'Di', 'San', 'Sisto', 'was', 'the', 'favourite', 'picture', 'of', 'the', 'Dresden', 'Gallery', 'The', 'little', 'sanctuary', 'where', 'the']
    Original number of words: 5776; Number remaining: 5834


You may have noticed that the number of words in our list is now greater than our starting number. This is a result of splitting by all punctuation--any contracted or hyphenated word has now been split into two. This is not an issue, as we'll remove these non-words later.

Let's now convert our words to lowercase:


```python
# Conver to lowercase with the 'lower' function
words = [i.lower() for i in words]

# Print the first 50 words to inspect: 
print("\n" + "First 50 words: " + "\n" + str(words[:50]))
```

    
    First 50 words: 
    ['i', '_', 'ome', 'comments', 'on', 'correg', 'ncrftjh', 'gio', 'in', 'connection', 'with', 'his', 'pictures', 'in', 'dresden', 'a', 'few', 'years', 'ago', 'it', 'would', 'have', 'been', 'hard', 'to', 'tell', 'whether', 'correggio', 's', 'night', 'or', 'raphael', 's', 'madonna', 'di', 'san', 'sisto', 'was', 'the', 'favourite', 'picture', 'of', 'the', 'dresden', 'gallery', 'the', 'little', 'sanctuary', 'where', 'the']


Let's try the spellchecker once more. And this time, we'll also use some additional functions within the SpellChecker module (including suggested corrections and adding words to the dictionary)


```python
# run SpellChecker on the first 100 items in our list of words and return only misspelled
misspelled = spell.unknown(words[:100])

# print the list of mispelled items from the first 100 words:
print("misspelled words: " + str(list(misspelled)))

# Print out a list of suggested corrected spellings:
for i in misspelled:
    print("original: " + i + "; suggested: " + spell.correction(i))
```

    misspelled words: ['sisto', 'ncrftjh', 'sixtus', 's', 'correggio', 'correg']
    original: sisto; suggested: visto
    original: ncrftjh; suggested: ncrftjh
    original: sixtus; suggested: situs
    original: s; suggested: i
    original: correggio; suggested: correggio
    original: correg; suggested: corre


Some of the false positives are proper nouns like "correggio", "sisto", and "sixtus". We can add these proper nouns to our dictionary, so they are no longer flagged by our spell checker. 


```python
# Add to dictionary the 'mispelled' names that are not such:
spell.word_frequency.load_words(['correggio', 'sixtus', 'sisto'])

# Rerun the spellcheck:
# run SpellChecker on the first 100 items in our list of words and return only misspelled
misspelled = spell.unknown(words[:100])

# print the list of mispelled items from the first 100 words:
print("misspelled words: " + str(list(misspelled)))


# This would run the spellcheck and remove all erroneous words (NOT USED)
# misspelled = spell.unknown(words)
# for i in mispelled:
#     words.remove(i)

```

    misspelled words: ['ncrftjh', 'correg', 's']


Better! If we had more time, we could explore the rest of the text and make an educated decision whether it was better to keep the misspelled words, or remove them (alongside a few false positives). We could also build a list of proper nouns that could be permanently added to a dictionary we could use for a related corpus.

## 6. Removing stop words with NLTK | Putting it all together
[NLTK](https://www.nltk.org/) is a common and very powerful NLP package that can perform any number of operations in an NLP "Pipeline". In this example, we're going to start with our original text and repeat much of our previous processes using the NLTK toolkit. We'll also remove all 'stop words' (like 'a', 'an, 'me', 'my', 'myself', 'we', 'our', 'ours', etc.) from our file, and filter out those pesky single-character words. 


```python
### import the word tokenizer and stop words from nltk, along with other modules that are required
import re
import nltk
import string
from nltk import word_tokenize
from nltk.corpus import stopwords
nltk.download('stopwords')
nltk.download('punkt')
stop_words = stopwords.words('english')

### Read the text file
with open('careggio-raw.txt') as f:
    text = f.read()

### Here we will regenerate tokens from our original text. We'll call them tokens
tokens_list = word_tokenize(text)
print("\n" + "First 50 tokens: " + "\n" + str(tokens_list[:50]))
orig_num_words = len(tokens_list) # count the original number of words in the text.

##########################
### Insert our filtering for caretted words: 
caret_words = list() # make a blank list that will hold our caret words.

# Iterate through the entire list of words. If a '^' exists, add it to our 'caret_words' list.
for i in tokens_list:
    a = re.search('[A-Z0-9]',i) # regular expression search for any capital letter or numeral
    b = re.search('[\^]',i)     # regular expression to search for a caret ('^') symbol
    if a and b:                 # An if statement that is true if both regular expressions find a match
        caret_words.append(i)   # if true, add to the list of caretted words to be removed
        tokens_list.remove(i)         # if true, remove the matching words from our list

        # Print the list: 
print("Caretted words removed : " + "\n" + str(list(caret_words)))

### convert to lower case
tokens_list = [i.lower() for i in tokens_list]

##########################
### Remove punctuation by making a replacment table between pucntuation and empty spaces
repl_table = str.maketrans('', '', string.punctuation)
tokens_nopunct = [i.translate(repl_table) for i in tokens_list]
print("\n" + "First 50 words after removing punctuation: " + "\n" + str(tokens_nopunct[:50]))

##############################
# Get rid of any other non-alphanumeric characters
words = [word for word in tokens_nopunct if word.isalpha()]

print("\n" + "First 50 words after removing non-alphanumeric: " + "\n" + str(words[:50]))

##############################
# Remove stopwords with nltk
words = [i for i in words if not i in stop_words]
print("\n" + "First 50 words after removing stop words: " + "\n" + str(words[:50]))

# Print an update on number of words remaining
print("Original number of words: " + str(orig_num_words) + "; Number remaining: " + str(len(words)))
```

    
    First 50 tokens: 
    ['i', '_', 'IPS^S^ffclS', 'OME', 'comments', 'on', 'correg', 'ncRftJH', 'GIO', 'IN', 'CONNECTION', 'with', '1^58^^^', 'HIS', 'PICTURES', 'IN', 'DRESDEN', '.', 'Spl^^T^ES', 'A', 'few', 'years', 'ago', ',', 'it', 'would', 'have', 'been', 'hard', 'u|^J^^fev^S', 'to', 'tell', 'whether', 'Correggio', "'s", 'Night', 'or', 'E^^g^^M', 'Raphael', "'s", 'Madonna', 'Di', 'San', 'Sisto', 'was', 'the', '^L^^^^^M', 'favourite', 'picture', 'of']
    Caretted words removed : 
    ['IPS^S^ffclS', '1^58^^^', 'Spl^^T^ES', 'u|^J^^fev^S', 'E^^g^^M', '^L^^^^^M', 'mmSSSmS^mSSm']
    
    First 50 words after removing punctuation: 
    ['i', '', 'ome', 'comments', 'on', 'correg', 'ncrftjh', 'gio', 'in', 'connection', 'with', 'his', 'pictures', 'in', 'dresden', '', 'a', 'few', 'years', 'ago', '', 'it', 'would', 'have', 'been', 'hard', 'to', 'tell', 'whether', 'correggio', 's', 'night', 'or', 'raphael', 's', 'madonna', 'di', 'san', 'sisto', 'was', 'the', 'favourite', 'picture', 'of', 'the', 'dresden', 'gallery', '', 'the', 'little']
    
    First 50 words after removing non-alphanumeric: 
    ['i', 'ome', 'comments', 'on', 'correg', 'ncrftjh', 'gio', 'in', 'connection', 'with', 'his', 'pictures', 'in', 'dresden', 'a', 'few', 'years', 'ago', 'it', 'would', 'have', 'been', 'hard', 'to', 'tell', 'whether', 'correggio', 's', 'night', 'or', 'raphael', 's', 'madonna', 'di', 'san', 'sisto', 'was', 'the', 'favourite', 'picture', 'of', 'the', 'dresden', 'gallery', 'the', 'little', 'sanctuary', 'where', 'the', 'virgin']
    
    First 50 words after removing stop words: 
    ['ome', 'comments', 'correg', 'ncrftjh', 'gio', 'connection', 'pictures', 'dresden', 'years', 'ago', 'would', 'hard', 'tell', 'whether', 'correggio', 'night', 'raphael', 'madonna', 'di', 'san', 'sisto', 'favourite', 'picture', 'dresden', 'gallery', 'little', 'sanctuary', 'virgin', 'saint', 'sixtus', 'floats', 'pseudoaltar', 'crowded', 'worshippers', 'correggio', 'picture', 'quite', 'large', 'devout', 'fol', 'lowing', 'change', 'popular', 'taste', 'evidently', 'taken', 'place', 'people', 'linger', 'night']
    Original number of words: 6576; Number remaining: 2805


    [nltk_data] Downloading package stopwords to /home/jovyan/nltk_data...
    [nltk_data]   Package stopwords is already up-to-date!
    [nltk_data] Downloading package punkt to /home/jovyan/nltk_data...
    [nltk_data]   Package punkt is already up-to-date!


Not bad! This final set is significantly reduced and much more normalized than before. There are still issues with erroneous words, and we would have to make a decision whether to further process the file to remove them, or if our analyses can tolerate a bit of inconsistency. 
  
But, we're now at the point where we can perform some initial analyses on the data. For example, let's list the 20 most common words in the text:  


```python
# import the pandas package
import pandas                       
from collections import Counter

# Take the word count using the Counter function
word_counts = Counter(words)
# Print it to the screen
print("Most common words and frequencies:")
word_counts.most_common(20)
#type(word_counts)
```

    Most common words and frequencies:





    [('correggio', 59),
     ('st', 51),
     ('picture', 39),
     ('dosso', 23),
     ('painter', 22),
     ('madonna', 21),
     ('like', 19),
     ('even', 18),
     ('francis', 18),
     ('light', 18),
     ('george', 16),
     ('one', 16),
     ('pictures', 15),
     ('first', 13),
     ('much', 13),
     ('little', 12),
     ('must', 12),
     ('colour', 12),
     ('years', 11),
     ('master', 11)]



## 7. Save your work
Let's write our winnowed list ```words``` to a comma separated value (CSV) file, so that we can save it and move it to other software, if desired. 


```python
# Use the 'write' 
with open("word_list.txt", "w") as outfile:
    outfile.write(",".join(words))
```

If you are runnign this on your local computer, your file will be saved in your working directory. 

If you are working within a Jupyter Notebook, you can view files by clicking on the Jupyter icon at the top of the notebook. <img src="attachment:image.png" alt="jupyter logo" width="80"/>

## 8. A little bit more fun
Finally, let's use [spaCy](https://spacy.io/)--another very powerful and full-featured NLP package--to see some of the higher-order analyses that such packages can perform. 

Here, we're going to use our text file to do some Named Entity Recognition (NER).


```python
### Read the text file
with open('careggio-raw.txt') as f:
    text = f.read()
    
# Import spacy and load the small pipeline
import spacy
nlp = spacy.load('en_core_web_sm')
# Run our text against the model
doc = nlp(text)

# Use the displacy module to display named entities
from spacy import displacy
displacy.render(doc,style='ent')
```


<span class="tex2jax_ignore"><div class="entities" style="line-height: 2.5; direction: ltr"> i _ IPS^S^ffclS 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    OME
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 comments on correg ncRftJH GIO IN CONNECTION with 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    1
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
^58^^^ HIS PICTURES IN DRESDEN. Spl^^T^ES 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    A few years ago
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
, it would have been hard u|^J^^fev^S to tell whether 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's Night or E^^g^^M 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Raphael
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna Di San Sisto
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 was the ^L^^^^^M favourite picture of 
<mark class="entity" style="background: #ddd; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the Dresden Gallery
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">FAC</span>
</mark>
. mmSSSmS^mSSm The little sanctuary where 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the Virgin with Saint Sixtus
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 floats above the pseudo-altar was then crowded with worshippers as it is now, and 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's picture had quite as large and devout a fol lowing. But some change in popular taste has evidently taken place, for few people now linger before the Night. What inference is to be drawn ? Was the enthusiasm for 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 merely a fashion which has had its season ? He is certainly no longer admired as he was in 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the first few decades of this century
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
, in 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the day
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
 when no gentleman could afford to be without his theory of the &quot; Correggiosity of Correggio.&quot; The explanation is not far to seek. The enthusiasm for Correggio dates from the time when, all the possible variations having been played upon the themes introduced by 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Raphael
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 and 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Michelangelo
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Caracci
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 betook themselves to a comparatively unlaboured field, and founded upon Correggio their school of painting, and thus succeeded in lending a new life to 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Italian
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 art. Most people, however, appreciate only what is of their own day, ana 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's in terpreters proved far more interesting to their contemporaries than the master himself. The 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Caracci
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Domenichino
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Guer
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 cino, 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Guido Reni
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, and 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Lanfranco
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 used up all the aesthetic capacity of their admirers, who believed in Correggio as the 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Catholic
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 peasant doubtless believes in God, although he makes his offerings to the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Saints
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
. Furthermore, it was by no means easy to know the master himself. Correggio lived to be scarcely 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    forty
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
. Of his pictures then known the earliest dates from 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    his twenty-first year
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
, and in a career of 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    barely twenty years
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
 no painter could have painted enough to fill the various collections of 
<mark class="entity" style="background: #ff9561; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Europe
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LOC</span>
</mark>
. But in 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the third decade of this century
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
, the few whose word was law in matters of taste sud denly turned away from 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Guido, Lanfranco
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, and their like, and gave themselves up to an unbridled enthusiasm for the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Caracci
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 and for their master, Correggio. Later, even the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Caracci
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 dropped out of sight, and Correggio stood alone. The 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 with 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    St. Francis
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, No. 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    150
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
. The 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 with 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    St. Sebastian
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, No. 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    151
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
. The 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Nativity
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, called the &quot; Night,&quot; No. 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    152
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
. The 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 with 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    St. George
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, No. 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    153
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
. 7373</br></br> The change was due in part to the fact that Correggio at last found in 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Toschi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, the engraver, a perfectly accurate trans lator and publisher. If engraving is considered as a fine art by itself, there have been many greater masters of the craft than 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Toschi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
; but no one ever assimilated more thoroughly than he the style of a great painter of 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    several centuries
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
 before, or ever gave such faithful, such impersonal renderings from an old master. When his engravings had made it really possible to know Correggio, the public placed him at once in the highest heaven. Nor, in this instance, were they wrong. The Correggio with whom they thus made acquaintance, the painter of the frescoes in 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the Convent of St. Paul at
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>

<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Parma
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
,? frescoes filled with delightful 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Cupids
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 playing hide-and-seek in garlands of flowers,? had a genius quite as fine as any artist of his time. 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Toschi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
's garlands and 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Cupids
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 must have been tantalizing to the lovers of 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, but the originals were far away. Fortunately there were several Correggios in 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dresden
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, which was near at hand, especially to the various seats of aesthetics, such as 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Jena
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, Weimar, and 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Gottingen
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
. In 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    one
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 of the 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dresden
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 pictures both the 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Cupids
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 and the garlands were to be seen, and this picture, the so-called 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    St. George
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, became at once a favourite, and of course a masterpiece. There is another reason, perhaps even more cogent, for the sudden popularity of the St. George. A change in taste is no more completed in 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    a day
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
 than a change in character. The St. George is one of 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Cor
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 reggio's latest paintings, and, as followers always take up a master's methods where he left them, this picture was one of those upon which the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Caracci
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 formed their own style. People were well acquainted with the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Caracci
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
: so they found it easy to appreciate a work in which Correggio differs but little from them. The St. George was painted 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    about 1532
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
, 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    two years
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
 before 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's death. It represents the 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 seated upon a throne of very barroque design, surrounded by 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    St. George
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 and other 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Saints
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
. Her face is flabby and puffy, though not with out a certain charm, and her eyes are very large. Her knees and her breast come close together: you can almost see the soles of her feet. To understand her appearance you must imagine yourself looking up at her from the bottom of a well. When Correggio painted this picture, he had already finished his frescoes in the dome of 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the Cathedral of Parma
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
. These are so full of movement, contain such startling feats of foreshortening, that the painter of them seems to have fallen a victim to the admiration of his own cleverness. At any rate, he never afterwards drew a figure in repose, or in a 7474</br></br> s normal position. He seemed to have used up his natural vein of feeling, and having used it up, his interest narrowed itself down to making his compositions animated and grandiose, debasing the human figure to purposes as vile as the con torted atlantides of barroque architecture. When this happens, when a painter begins to think of experimenting with the lines of the human figure as something merely &quot; composable,&quot; instead of regarding them as a means of expression; or, worse still, when he is possessed by the desire to exhibit his clever ness, there is little further to be said about him as an artist. If the St. George were well preserved, which is far from being the case, one might say, in the language of the 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Parisian
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 ateliers, &quot; Cest trte chic, ca.' 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Chic
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, hollow cleverness, is all that the picture can be said to have; it must be added, however, that it has this to a degree that makes it really enjoyable. But the picture had a further charm which appealed to the connoisseurs of the day. To understand it, we need an idea of the dominant spirit of 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    German
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 literature during the twen ties. Its principal exponents were 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Novalis
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Fouqu6
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Tieck
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, and the Scnlegels. Goethe's reign had passed, 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Heine
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's had not yet begun. It was 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the decade
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
 of pure Romanticism, which, in 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Germany
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, was by no means that innocent, rough and-tumble movement of liberation from prim manners and bad couplets it was in 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    England
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
. The heroine of Miss 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Austen
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's &quot; 
<mark class="entity" style="background: #f0d0ff; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Northanger Abbey
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">WORK_OF_ART</span>
</mark>
&quot; is a much better representa \ tive of 
<mark class="entity" style="background: #ff8197; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    English
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LANGUAGE</span>
</mark>
 romanticism than the somewhat hectic 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mari
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 anne of &quot;
<mark class="entity" style="background: #f0d0ff; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Sense and Sensibility
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">WORK_OF_ART</span>
</mark>
.&quot; But even 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Marianne
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 would have seemed a very coarse creature to the cultivators of 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the &quot; Blue Flower
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
.&quot; (
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    German
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 romanticism carried sensibility to the point where it becomes insanity. Health was thought vulgar. It was the fashion to seem diaphanous, to cough tellingly, to look worlds, and to take the greatest interest in what was of no human concern. The St. George in 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correg
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 fio's picture was not diaphanous or consumptive, to be sure, ut he certainly had the morphine habit, which would make him quite as interesting. His foot rests upon a dragon, no doubt a symbol of his victory over the demon of 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Morphia
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
. The children, with their huge heads and watery eyes, are monsters that might have been suggested by some fantastic tale of 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Hoffman
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's. St. 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Peter Martyr
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 talks theology with the sincerity of 
<mark class="entity" style="background: #f0d0ff; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    August von Schlegel
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">WORK_OF_ART</span>
</mark>
, and 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    St. John
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 is sufficiently like 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Antinous
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 preparing for a ballet to have rejoiced 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Wilhelm von Schlegel
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Gottingen
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 professor of aesthetics. The only figure in the picture that has any health in him is St. Gemig niano, and he hides in the background as if ashamed of being 7575</br></br> so robust. For these reasons, then, the St. George became the standard, the canon of Correggio, and his other pictures were judged accordingly. The Night, being nearest to it, came next in public favour. Indeed, when Romanticism began to go out of fashion, it became the supreme favourite. This altar-piece was painted 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    at least two years
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
 before the St. George. The subject is the 
<mark class="entity" style="background: #ff9561; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Nativity?a
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LOC</span>
</mark>
 subject so of 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    ten
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 Eaintea that Correggio might well have asked himself how e was to avoid the commonplace in treating it once more. I am inclined to think that the painter tried to interpret the divine event from a point of view as human and lowly as that of the 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Gospels
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 themselves. The 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 is in the 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    first
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORDINAL</span>
</mark>
 place a young mother, the 
<mark class="entity" style="background: #ff8197; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Child
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LAW</span>
</mark>
 is a mere human infant, and the Shepherds are nothing but shepherds. 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    St. Joseph
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, instead of being the usual supernumerary, is occupied in leading away a mule, who lingers, attracted by the light, or perhaps oy the straw. There is no conventional choir of angels. His angels are too wild with joy to pose languidly with mandolins in their hands. It was the sneer humanity of this picture that drew so many pilgrims to it, and not, as the critics of that time said, because Correggio had the wonderful idea of mak ing all the light stream from the 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Infant
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's face. Correggio may have had some such purpose, only as an intention it is rather literary than pictorial, and it is more likely that he had something in mind far less theological and poetical. His idea seems to have been to experiment with lights. From the 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Child
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's face the light streams out into the darkness, and dies away just before it encounters the 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    first
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORDINAL</span>
</mark>
 white of dawn appear ing over the horizon. In the present condition of the picture, it is no longer possible to judge what was the effect. That it must have been very wonaerlul there can be no doubt. But even if the effect of the meeting of 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    half
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 lights and reflected lights at a point darker than either could still be appreciated, it would remain true that not the lights, but the human inter pretation of the subject, made the Night so great a favourite. A real feeling for artistic treatment as distinct from illustra tion must now be much more wide-spread than it used to be, otherwise it is hard to explain why the Night has so fallen from grace. If the appreciation of painting has increased to such a de gree that it is no longer possible to be very enthusiastic about these 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    two
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 pictures, once so much admired, does it mean that there is nothing left to enjoy in Correggio ? As the knowledge of the old masters advances, and as, with the aid of discriminating criticism, the power to enjoy them 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    7676
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
</br></br> increases, it is found more and more that the latest works of a painter are not necessarily his best. Indeed it seems that many of the 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Italian
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 masters were most fascinating soon after they began to paint, or at any rate, while they were still young. This was especially true of painters such as Correggio who were more sensitive, perhaps, than vigorous?lyric rather than epic. The 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dresden
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 gallery possesses a picture by Correggio which the leaders of aesthetics in 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the early decades of this century
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
 scarcely deigned to notice. It was done in 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    1514
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
, in 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    his twenty first year
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
, and being so early a work, it deserves careful atten tion. It represents the 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 with the 
<mark class="entity" style="background: #ff8197; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Child
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LAW</span>
</mark>
 in her lap, enthroned under an arch, with 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    four
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>

<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Saints
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 at her feet, 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    St. Catherine
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 and 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    John the Baptist
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 on her left, and 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    St. Francis
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 and 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    St. Anthony
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 of 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Padua
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 to the right. Above, just under I the arch, 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    two
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 little angels are poised in the air, as if it were i water in which they floated joyfully at their ease. Between them is a halo, or glory, with ruddy cherubs peeping out from f| the straw-coloured light. The little angels are restful, the 
<mark class="entity" style="background: #ff8197; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Child
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LAW</span>
</mark>
 is quiet and simple, as different as possible from the ff nervous imp in the St. George. The 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 has a face in which there is nothing mystifying, nothing theatrical. The Eoses of the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Saints
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 are dramatic, their interest is very quick; ut they are not at all so melo-dramatic as even 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Raphael
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's St. ?
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    51
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 I?*111 *n 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the Madonna Degli Ansidei
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, in 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the National Gallery
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
. %';'; ; The scheme of colour is bright and clear, but quiet. The arch behind the throne opens upon an unobtrusive landscape. Although this is not among the severest nor yet among the rl most majestic altar-pieces ever painted, it is one of the most - |; delicate and most felt. Modern criticism, then, takes this 
<mark class="entity" style="background: #ffeb80; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    II
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">EVENT</span>
</mark>
 early work as a starting point for the study of Correggio. A W few years ago it was supposed, indeed, to be his earliest paint W-\ ing; but to-day we know nine or ten which he must have done i before. But as it was the study given to the St. Francis which led to the discovery of the earlier paintings, we can do nothing better than stop here and see what this picture can reveal of 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
's history. You cannot look long at the picture without the haunting feeling of having seen elsewhere something very much like it. The truth is that the St. Catherine here bears a strong resem blance to 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Raphael
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's St. Catherine in 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the National Gallery
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, and 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the St. John
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 is not unlike the St. John in his 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna Di Foligno
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, of the 
<mark class="entity" style="background: #ddd; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Vatican
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">FAC</span>
</mark>
. Other resemblances might be traced; but these are enough. The phenomena of art are as certainly governed by laws as 7777</br></br> the phenomena of nature. The province of art criticism is to discover these laws, which prove that in art, as in nature, there is no such thing as mere coincidence. Striking resemblances such as these must be accounted for. It is of course out of the question that Correggio was the pupil of 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Raphael
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
; but recent researches have given us the clue to the real connection between them. It is now admitted that 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Raphael
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 owes much of what is characteristic in his style to nis 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    first
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORDINAL</span>
</mark>
 master, 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Timoteo Viti
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, and, through him, to 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Francesco Francia
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 and 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Lorenzo Costa
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Timoteo
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
's teachers at 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Bologna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
. Correggio's training has been traced to the same sources, for he also was, directly or indirectly, the pupil of 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Costa
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 and. Francia. On the wall opposite to the 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    St. Francis
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 hangs a characteristic picture by 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Francia
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
? The Baptism of Christ. The moment your attention is called to it, you perceive that the movement of the figures in both pictures is strikingly the same; that the eyes in both are opened wide in the same way, and that the general scheme of colour is not unlike, the reds and yellows, indeed, being identical. Correggio's is finer subtler, more delicate, but they differ only as members of the same family. The influence of 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Francia
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, however, does not explain every thing in Correggio's picture. The 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's face, for in stance, suggests 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Costa s
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 type, rather than 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Francia
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's, and the figure of Moses in the medallion on the throne is taken almost direct from 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Costa
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
. The medallion itself, the elaborate throne, the bas-reliefs on its base, are all of them peculiar to 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Costa
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 and to the 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Ferrarese
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 school from which he came; and their presence in a picture by 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 is sufficient ground for placing him among the painters of the school of Ferrara. But Correggio used to be described as a Lombard painter whose 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    first
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORDINAL</span>
</mark>
 master was a certain 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Bianchi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, but who owed his real training to 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mantegna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
. Mantegna, however, died in 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    1506
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
, when 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 was scarcely 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    twelve years old
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
. At that age, precocious even as he was, he could scarcely have done more than acquire the rudiments of his craft, and it is not likely that instruction received so young would have finally deter mined his style. Nor, indeed, is there much in the St. Francis to indicate a personal relation between Correggio and Man tegna. The pose of the 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, to be sure, is taken from 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mantegna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
's 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Vierge Des Victoires
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 now in the 
<mark class="entity" style="background: #ff8197; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Louvre
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LAW</span>
</mark>
, which 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Cor
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 reggio could have seen nowhere else than at 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mantua
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
. Poses, however, or even whole episodes, borrowed from another painter, no more prove direct descent than, for instance, the 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Latin
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 words we nave borrowed prove that 
<mark class="entity" style="background: #ff8197; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    English
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LANGUAGE</span>
</mark>
 was de rived from 
<mark class="entity" style="background: #ff8197; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Latin
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LANGUAGE</span>
</mark>
. Although there is no ground, therefore, for 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    7878
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
</br></br> the belief that Correggio was a pupil of 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mantegna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, the St. Francis confirms the old theory about 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Bianchi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
. The young painter was taught, just as children are taught writing, to draw the human form after a set fashion, especially such parts as the hands and ears, which are very obvious, and yet, when looked at carelessly, without much individuality. Naturally the method was the master's own, and the pupil kept it all his life, in spite of gradual and great variation. This fixed manner of drawing the hands and ears often assists us greatly in seeing the connection between a painter, his master, and his pupils, or in determining the authorship of a picture. You cannot look at the hands in the St. Francis with out noticing not only that they are well modelled, that they are refined, but that the 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    second
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORDINAL</span>
</mark>
 finger of each is too long. Even if this were a personal idiosyncrasy, it is not an accident, for it is found in most of 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
's pictures. Bianchi, whom the legend mentions as 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's teacher, is now being re discovered after having been almost forgotten for 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    centuries
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
. His masterpiece, whicn shows that he was a painter of the 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Ferrarese
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 school, is a {
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 and 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Saints
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 in the &quot;Louvre. Few pictures even of that wonderful collection can surpass it for grandeur of composition, subtlety of feeling, clearness of colour, and quietness of tone. The flesh colour is very white. Make it less smooth, model it a little more, and you have the unrivalled flesh of the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    tAntiope
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
's greatest achieve^ ment in flesh painting. The hands in this picture by 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Bianchi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 are not only shaped somewhat like the hanas in the St. 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Francis
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, but they have also the same characteristic, the elongated 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    second
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORDINAL</span>
</mark>
 finger. This goes a great way to prove that the legend was right in calling 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Bianchi Correggio's
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>

<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    first
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORDINAL</span>
</mark>
 master. Another thing that a pupil learned to do after a set fashion was the landscape. In the St. 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Francis
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 the landscape is in tone and colour, if not altogether in drawing, identical with that in 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Bianchi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
. Indeed, Correggio, allowing for the ad vances he made of himself in the differentiation of light, and in aerial perspective, remains always true to this type. In tone and colour the landscape even of the St. George, painted, it will be remembered, 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    two years
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
 before his death, is much like 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Bianchi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's. Correggio probably left 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Bianchi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 and went to Francia and Costa at 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Bologna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 in 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    1508
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
, when he was 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    fourteen years old
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
. In 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    1509
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>

<mark class="entity" style="background: #ff9561; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Costa
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LOC</span>
</mark>
 went to 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mantua
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 to take the place left vacant by 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mantegna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, and there is good reason to believe that 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Cor
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 reggio went with him and remained there for 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    several years
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
. We have already seen that the St. 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Francis
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 proves his presence 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    7979
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
</br></br> at 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mantua
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 before 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    1514
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
, and a still earlier picture, belonging to 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Signor Crespi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 of 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Milan
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 confirms the proof; for 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    two
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 of its figures ?St. 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Elizabeth
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 and 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the Infant John
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 ?are taken di rectly from 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mantegna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
's picture which still hangs in his mort uary chapel at 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mantua
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
. 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's stay in 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mantua
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 brought him in contact with a painter under whose inspiration nis work took on a character which was altogether more modern than 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Costa
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
's. This painter was 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso Dossi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, who was in 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mantua
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 in 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    1511
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
 and 1512. His influence can be traced in the whole series of 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
's early pictures which ends with the St. Francis. In this altar-piece the peculiar colour of the halo, like pale sulphur, and the ruddy cherubs which frame it in, are strikingly like the halo and the cherubs in a small 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Corona
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 tion of the Virgin by Dosso, which is also in the 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dresden
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 gallery. The little angels below the globe in 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
's picture are posed in a way which suggests at once the pose of the Christ Child in the St. 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Francis
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, ana in nearly all the early Correggios?in the Holy Family of Hampton Court, in the 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 of the Muni cipal 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Museum of Milan
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, in the 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 and 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Saints
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 belonging to 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Signor Frizzoni
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, also in 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Milan
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, in the 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 at 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Pavia
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, and in the (
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 with Angels of the 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Uffizi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
. It would not be hard to weave a romance about Correggio's relation to 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
. In 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    1511
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
 Dosso Dossi was 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    thirty-two years old
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
, and nearly at the height of his genius. It was just before he became the court painter of Alfonzo of Ferrara and 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Lucre tia Borgia
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, his wife. 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Ariosto
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 speaks of him in his 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Orlando
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 along with Giorgione and Leornardo and 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Michelangelo
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
. The court poet and the court painter were remarkably alike in the essence of their genius. They were both lovers of &quot; high romance,&quot; and both had the power to create it?the one in verse, the other in colour?with a splendour that per haps many other 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Italians
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 could have equalled, but with a fantasy, a touch of magic, that was more characteristic of 
<mark class="entity" style="background: #ff8197; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    English
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LANGUAGE</span>
</mark>
 genius in the 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Elizabethan
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 period than of 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Italian
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 genius at any time. Real feeling for the fantastic and magi cal was not often granted to the well-balanced 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Italian
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 mind. It is all the more delightful, therefore, to find an artist who has not only the strength and self-possession of an 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Italian
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
, but the romance and sense of mystery of the great 
<mark class="entity" style="background: #ff8197; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    English
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LANGUAGE</span>
</mark>
 poets. If 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Marlowe
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 had written about 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Circe
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, he would have presented us with 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    one
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 like 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's, as she may be seen in the 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Borghese
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 gallery: an enchantress clothed in crimson and emerald, sitting under balsamic trees where olive green lights are playing, with the monsters about her feet, their real na tures made visible by her arts. Rather than 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Greek
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 she is 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    8080
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
</br></br> 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Arabian
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
. She gives us no time to ask whether the lines of her form are classical, or whether her form is statuesque. Before her we lose ourselves in a maze of strange lights and mysterious colours which make us sink deeper and deeper into a world which is as entrancing as it is far away. Mar : lowe and 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Shakespeare
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 would have taken that delight in her which we can well imagine 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Ariosto
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 took. The painter of pictures like this could not help having an extraordinary fascination over such a sensitive, dreamy, / ecstatic temperament as 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's. It is easy to imagine : the precocious lad of 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    sixteen
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
, with his training already far advanced, but with faculties interpretive rather than creative, \ falling down in worship before the dazzling achievements of the 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Ferrarese
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 painter. 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's personal charm, also, must have been great, and he was just at the point in his life when the man is the boy's ideal. Fellow artists, then as now, we | may be sure, talked of nothing so much as of their craft. 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    k Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, who had been in contact with the 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Venetians
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
, and with pupils of 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Raphael
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, was in touch with many of the problems ^ that interested the painters of 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the day
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
. Nothing occupied % the best painters just then so much as the problem of light ? the eternal problem of painting. In Florence, Pier dei ?t 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Franceschi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Verrocchio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, and 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Leonardo
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
; and in 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Venice
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Alvise
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 r 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Vivarini
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Carpaccio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 and 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Giorgione
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 had brought the treat-. |V ment of light to a point far beyond anything 
<mark class="entity" style="background: #ff9561; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Costa
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LOC</span>
</mark>
 could | have known. It was just this advanced treatment which was I necessary to give Correggio the means to develop his peculiar f talent; for he was not aestined to create new types or new Va subjects. It was his destiny rather to be among the 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    first
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORDINAL</span>
</mark>
 to I treat his subject for the personal feeling and not for the mere v action ? still less for the mere composition. When he is at ; his best, he not only makes the face but the whole figure, and the landscape as well, the vehicle of emotion, and this to such \&quot; a degree that to go beyond is to become 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    a Guido Reni
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
. But he - never could have accomplished this with the limited acquaint ance with light his first masters gave him. From Dosso he : got the impulse for that study of the effects of light, which r itself became in his hands a means of expression utterly 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    un
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 dreamt of heretofore. It is easy to trace this connection between them. In almost every picture of 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's, where the subject and composition permit (as, for instance, in the small 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Coronation
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 already men tioned), the groups are so arranged that in looking at the land scape, one seems to be looking out upon it from within a cavern. This is the case to an even greater degree in a larger 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    81
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 *
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    81
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
</br></br> 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Coronation
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 by Dosso, which hangs opposite to the St. Francis. Where this cavern-like effect is not possible, 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 used to light one part of his picture much more than the rest, as one may see in his 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    St. George
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, which hangs above the St. Francis. In short, his pictures never present the appearance of the greater part of the paintings done before 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    his day
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
,? the appearance of an infinitesimally low relief. On the contrary, he batters great holes into his compositions, huge pockets, as it were, and fills them with light. To make the contrast even greater, he gives a slate-gray colour to the rim of this well of light, if not to the whole darker portion of the picture. Correggio took this treatment from 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, refined and advanced upon it, but 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's treatment of light and shadow contains in embryo all Correggio's. The 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    first
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORDINAL</span>
</mark>
 instance of this in Correggio occurs in a picture already mentioned, belonging to 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Signor Crespi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 of 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Milan
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
. It is a Nativity. The Holy Child is lying in a wicker basket on the ground. The Virgin kneels beside Him. 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Two
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 little angels are floating above, like the angels in the St. Francis. To the left, from the direction where the light is breaking, 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    two
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 other angels lead up the pious shepherds. Not only does the light in this picture all stream from 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    one
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 corner, but the general tone is slate-gray. Looked at merely as light and tone it is identical with 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
's 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    St. George
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
. It is easy to see the same influence in many other details, as, for instance, in the drawing of the eyes and mouths, which 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, in his works, was inclined to make like black holes. This peculiar way of making the eyes and mouths, transmitted to Correggio, is even more clearly visible in a picture belonging to Mr. 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Robert Benson
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 of 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    London
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
. It must have been painted somewhat later than the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Nativity
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
. The subject is Christ taking leave of His Mother. Clothed in white, He kneels with crossed arms at His mother's feet. She seems to be on the point of rushing for ward to embrace Him, but is held back by 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mary
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
. The Virgin's face expresses the greatest grief, but nothing wild or 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    un
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 seemly. 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    St. John
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 stands a trifle back, with his hands clasped in sympathizing sorrow. The distribution of light here is even more 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dossesque
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 than in the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Nativity
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
. It all comes from the left hand corner, whence it breaks over the Lake of 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Gali lee
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, broods over its surface with a pale gray light, flashes up to the sky in a greenish streamer, and is reflected on Christ's raiment, and on His mother's face. So we might take up 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
's earliest paintings, one after another, and find Dosso in them all. I must mention 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    one
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 more at all events. It is an Epiphany, in the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Archbishop
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
's 
<mark class="entity" style="background: #bfeeb7; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    8282
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PRODUCT</span>
</mark>
</br></br> 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    i?i
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
&quot;&quot; ' * ? palace in 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Milan
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
. At 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    first
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORDINAL</span>
</mark>
 glance, it is hardly to be 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    distin
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 ffuished from a 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
; but a more careful examination leaves ittle doubt that it is by the younger painter, who, in this in stance, seems to have caught, along with 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's way of painting, something also of his feelW for the romantic. 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's influence comes out again in the 
<mark class="entity" style="background: #ddd; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    T{est
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">FAC</span>
</mark>
 in the Flight, Eainted a little before the St. Francis. Of the 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    three
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 pictures y Correggio that the 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Uffizi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 possesses, this is by far the most beautiful. The deep-set eyes, the pale sulphur colour in the 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's robe, the dark wood, are all reminders of 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, and the 
<mark class="entity" style="background: #bfeeb7; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Virgin
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PRODUCT</span>
</mark>
 herself might be half-sister to the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Circe
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
. Having deducted the story of 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
's growth bit by bit, it will perhaps not be found amiss to sum it up briefly. 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Cor
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 reggio got his rudiments from 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Bianchi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, who handed him on to 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Francia
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 and 
<mark class="entity" style="background: #ff9561; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Costa
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LOC</span>
</mark>
. Costa took him to 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mantua
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, where the works of 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Mantegna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 seem to have made only a passing impres sion on him. There he came in close contact with 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso Dossi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, who helped him to acquire a method of painting which gave full scope to his genius. But to complete the account of the influences which went to form him, it remains for me to speak of a picture in Munich?the little Faun?so strikingly 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Venetian
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 in character that 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dosso
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
's influence alone will not ex plain it. In colour it is like a 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Palma
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, in movement it is like a 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Lotto
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
. It leads inevitably to the inference that Correggio must have visited 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Venice
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 before finally settling down near 
<mark class="entity" style="background: #ff8197; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Parma
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LANGUAGE</span>
</mark>
 ? an inference that might explain the puzzling like nesses between 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Lotto
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 and Correggio that keep forcing them selves upon our attention. Art is a flower of the human personality. Flower-like, it breathes out perhaps not its strongest, but often its most deli cate, perfume soon after bursting. It is delicious to catch an artist's naive impressions of the world, and this is one of the rewards of studying the earliest works of a painter. In the Italian 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Renaissance
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, at least, if a man was born with some thing to say in form and colour, he was likely to say the best of it very soon after he had fair mastery of his brush, rather than later, when manifold commissions, family concerns, and the ever advancing invasion of the commonplace, made him think of his painting less as an art and more as a business. This is above all the case with Correggio, whose genius was so distinctively subjective and lyrical. The pictures al ready discussed bring out this point clearly. The briefest comparison, for instance, between 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Signor Crespi's
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 [h{ativitv, painted when 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Correggio
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 was 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    sixteen
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
 or 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    seventeen
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
, and the same subject as treated in the Slight, when he was 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    thirty-five
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
, 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    83
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 4?83</br></br> shows that although he had made immense technical ad vances in the later picture, he had lost that intense and poeti cal religious feeling which made the early picture so impres sive. Or, again, the Rest in the Flight of the 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Uffizi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
 has a per sonal quality which somehow the later picture of the same subject at 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Parma
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, beautiful as it is, utterly lacks. In the 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    first
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORDINAL</span>
</mark>
, you feel that Correggio tried to live the scene before painting it; in the other, that he is reciting it like a lesson well learned. Even in 
<mark class="entity" style="background: #ff9561; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    the St Francis
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LOC</span>
</mark>
, he had lost something of the religious imagination he had when he was at work on the picture of Christ taking leave of His Mother. There he had an intensity of feeling and a reserve of expression which we no longer find in the later picture, where the feeling is much more ordinary and the expression at the same time a little exaggerated. Another advantage of studying a painter from his begin nings arises from the fact that there is a large intellectual ele ment even in pleasure supposed to be purely aesthetic. A Eainter naturally shows more clearly at nrst than afterwards 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    ow
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 he is connected with the other painters of his time. We get to know his 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    first
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORDINAL</span>
</mark>
 forms, to see how they have come down to him from his teachers, and, finally, how they are trans mitted by him to his pupils. If we learn to like his charac teristics tor their beauty and for their historical associations, we continue to like them, no matter how disguised. The ability to trace them in a picture makes that picture to some degree delightful in spite of the faults it may otherwise have. It is, for example, a pleasure to find in the St. George that the landscape and the hands still retain something of their early likeness to 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Bianchi
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
. Although there is little left to enjoy in the Night, yet an acquaintance with 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Signor Crespi's
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>

<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Nativity
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 makes it most interesting to see how the painter treated the subject after 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    a lapse of twenty years
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
. So that the study of the early works of a master not only reveals him at a period when he is likely to be very charming, but makes him inter esting even in his decadence. It has not been my purpose to speak of the works of 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Cor
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 reggio's maturity. In the period between the St. Francis and the St. George, he painted such pictures as the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Antiope
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 and the 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Leda
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
, the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Danae
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, the lo, and the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Ganymede
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, pictures as intensely lyric as his earliest are sincerely religious. It only remains to mention another picture in the Dresden Gallery dating from this same period. It is the so-called St. Sebastian ? the 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 on a throne with cherubs and angels about her, looking down upon St. 
<mark class="entity" style="background: #c887fb; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Sebastian
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">NORP</span>
</mark>
 and 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    two
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
 other saints. It is. a picture with the same movement and the same feeling that 8484</br></br> we find in the 
<mark class="entity" style="background: #ddd; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Leda
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">FAC</span>
</mark>
, the 
<mark class="entity" style="background: #ff9561; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Io
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LOC</span>
</mark>
, or the 
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Danae
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
, even the face of the 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Madonna
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 being very much like what the 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Leda
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
 must have t\: been. Like all these masterpieces it is full of that high-strung f-&gt; sensuous emotion which inevitably suggests the music of violins. But of course such movement and such feeling are \' utterly out of harmony with the subject of a religious picture, where the effect to be produced is one of awe and devotion, not of fellowship with the gods in ecstatic enjoyment. 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Dresden
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    October, 1890
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
. 
<mark class="entity" style="background: #aa9cfc; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Bernhard Berenson
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PERSON</span>
</mark>
. Florence, 
<mark class="entity" style="background: #bfe1d9; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    February
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">DATE</span>
</mark>
, 
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    1892.85
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">CARDINAL</span>
</mark>
</br></br></br></div></span>


## 9. Wrap-up
Although these exercises provided only the most preliminary introduction to text preparation for analysis and dissemination, we hope that it gave you a sense of the broad potential for using programmatic approaches to do so. To explore some next steps, refer to the [Learn More](https://jasonbrodeur.github.io/dsi-text-prep/learn-more.html) section of the workshop website. 
