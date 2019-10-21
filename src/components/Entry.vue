<template>
  <div>
  //shortening function for the name
    var nameInsert = underXXCharacterCheck(novel[i].name, characterLimiter);
    // if a name is to short, it gets a
    <br />element to have constant height everywhere
    nameInsert = (nameInsert.length < characterMinimum) ? nameInsert + "
    <br />
    <br />" : nameInsert;
    //the slider elements
    var novelDIV = document.createElement("div");
    novelDIV.setAttribute("class", "novel");
    // the elements structure
    novelDIV.innerHTML = `
    <h4 class="title uk-heading-line uk-text-center">${nameInsert}</h4>
    <div>
      <img src="${novel[i].picSource}" alt="${novel[i].name} Cover" class="cover" />
      <h5 class="ranking uk-text-center">Rating: ${novel[i].ranking}/10</h5>
      <button
        uk-toggle="target: #id${novel[i]._id}"
        class="uk-button uk-button-primary modalButton"
      >More Information</button>
    </div>`
  </div>
</template>

<script>
export default {
  name: "Entry",
  props: {
    
  },
  methods: {
    content() {
      return [
        {
          Title: "Physicians Odyssey ",
          Description:
            "An urban fiction. Mediocre is a common characteristic of the majority. But when someone outstanding appears beside you, your life will change because of him. That’s the principle of ‘who you mix around with is a reflection of yourself.’ The doctor in charge of the Three Flavour Hall will change everything. You can raise a campus belle, a CEO beauty…",
          Date: "13 10.2019",
          Origin: "chinese",
          Rank: "9",
          Link: "https://www.novelupdates.com/series/physicians-odyssey/",
          Picture:
            "https://www.asianovel.com/img/novel/cover/default/full/physicians-odyssey.jpg",
          Dropped: "FALSE",
          Progress: "C192"
        },
        {
          Title: "Scholar's Advanced Technological System",
          Description:
            "After suffering from a heat stroke while working under the scorching heat of summer, Lu Zhou, a hardworking but poor university student, somehow becomes the owner of an advanced technological system. With the cheat given by the system, his university life changes overnight.",
          Date: 43749,
          Origin: "chinese",
          Rank: "8",
          Link:
            "https://www.webnovel.com/book/12583970906002305/Scholar's-Advanced-Technological-System",
          Picture:
            "http:////img.webnovel.com/bookcover/12583970906002305/600/600.jpg",
          Dropped: "FALSE",
          Progress: "C222"
        },
        {
          Title: "Good Girl Gone Bad: Queen of the Underworld",
          Description:
            'After her gruesome death, Seraphina Yue woke up 6 years in the past.  "Bloody hell! Did I survive hell?"',
          Date: 43734,
          Origin: "webnivel",
          Rank: "6",
          Link:
            "https://www.webnovel.com/book/11911622506604705/Good-Girl-Gone-Bad%3A-Queen-of-the-Underworld",
          Picture:
            "http:////img.webnovel.com/bookcover/11911622506604705/600/600.jpg",
          Dropped: "TRUE",
          Progress: "C63"
        },
        {
          Title: "The Divine Anime System",
          Description:
            "Every day life was monochromatic for Seig, the people passing him looked the same. No colours. No sound. Every thing was dreary. He was deemed an outcast as his vacant look in his eyes makes him seem dead. Until, his maid showed him anime series. It was an explosion of colours which brought life into his eyes. From then he had always wanted to be like the main characters, making viewers look in awe as they show off their powers. Even when he was bullied for being liking anime he still kept a positive outlook by watching anime. It all paid off when he died a hero and was rewarded with something to help him achieve his dream in another world.",
          Date: 43733,
          Origin: "webnovel",
          Rank: "4",
          Link:
            "https://www.webnovel.com/book/12114214905945305/The-Divine-Anime-System",
          Picture:
            "http://img.webnovel.com/bookcover/12114214905945305/600/600.jpg",
          Dropped: "TRUE",
          Progress: "C41"
        },
        {
          Title: "NoGift",
          Description:
            "She's a known genius of the 31st century. But due to humans' endless greed, she was forced to end her life. The moment she decided to die, she already accepted that it would be the end. But miraculously, she survived and woke up in a completely different world. A world filled with magic and creatures that she thought only existed from myths and legends. The people there have abilities called 'Gifts'. There's only one problem tho. She's now inhabiting a different body. And that body belongs to a... young duke?",
          Date: 43723,
          Origin: "webnovel",
          Rank: "9",
          Link: "https://www.webnovel.com/book/11653172006457405/NoGift",
          Picture:
            "http://img.webnovel.com/bookcover/11653172006457405/600/600.jpg",
          Dropped: "FALSE",
          Progress: "C107"
        },
        {
          Title: "Chrysalis",
          Description:
            "Anthony has been reborn! Placed into the remarkable game-like world of Pangera. However, something seems a little off. What's with these skills? Bite? Dig? Wait.... I've been reborn as a WHAT?! Follow Anthony as he attempts to adjust to his new life, to survive and grow in his new Dungeon home!",
          Date: 43721,
          Origin: "webnovel",
          Rank: "9",
          Link: "https://www.webnovel.com/book/11236791006300205/Chrysalis",
          Picture:
            "http://img.webnovel.com/bookcover/11236791006300205/600/600.jpg",
          Dropped: "FALSE",
          Progress: "C309"
        }
      ];
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">

</style>
