<template>
  <!-- This is the modal with the default close button -->
  <!-- modalIdMaker is used to convert the title of the novel into a usable id -->
  <div uk-modal :id="'modal' + modalIdMaker(novel.Title)">
    <div class="uk-modal-dialog uk-modal-body uk-text-center">
      <button class="uk-modal-close-default" type="button" uk-close></button>
      <h2 class="uk-modal-title">{{novel.Title}}</h2>
      <h4 class="bottomMargin">Author: {{novel.Author}}</h4>
      <div class="uk-flex uk-flex-around uk-flex-wrap">
        <div>
          <h4>
            Entry on
            <b>{{novel.Date}}</b>
          </h4>
        </div>
        <div>
          <h4>
            Status:
            <!-- changes the status entry in the database which is a boolean to text -->
            <!-- conditional css selector -->
            <span
              :class="novel.Dropped ? labelDanger : labelSuccess"
            >{{droppedToTextConverter(novel.Dropped, novel.Finished)}}</span>
          </h4>
        </div>
        <div>
          <h4>
            Origin:
            <span class="uk-label tableLabel">{{novel.Origin}}</span>
          </h4>
        </div>
      </div>
      <img
        rel="preconnect"
        :src="novel.Picture"
        :alt="novel.Title + ' Cover'"
        class="modalCover"
      />
      <div class="uk-flex uk-flex-around uk-flex-wrap">
        <div>
          <h4 class="noBottomMargin">
            Progress:
            <b>{{novel.Progress}}</b>
          </h4>
        </div>
        <div>
          <h4 class="noBottomMargin">
            Rank:
            <!-- uses the label design for the ranking for better visual -->
            <span class="uk-label tableLabel">{{novel.Rank}}</span>
          </h4>
        </div>
      </div>
      <h4>Review Statement</h4>
      <p class="modalDescription">{{novel.Review}}</p>
      <h4>Description</h4>
      <p class="modalDescription">{{novel.Description}}</p>
      <a :href="novel.Link" target="_blank">
        <button class="uk-button uk-button-primary modalLinkButton">Novel Link</button>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: "Modal",
  props: { novel: Object },
  data() {
    return {
      //for conditional randering of css
      labelDanger: "uk-label uk-label-danger tableLabel",
      labelSuccess: "uk-label uk-label-success tableLabel"
    };
  },
  methods: {
    //makes title to id
    modalIdMaker(input) {
      return input.replace(/\W+/g, "");
    },
    //converts dropped boolean to text
    droppedToTextConverter(status, finished) {
      if (status) {
        return "Dropped";
      } else if (status && finished){
        return "Finished";
      } else {
        return "Reading";
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.uk-label {
  font-size: 80%;
}
.bottomMargin {
  margin: 0 0 20px 0;
}
//picture
.modalCover {
  margin-bottom: 1em;
  height: 400px;
}
.noBottomMargin {
  margin-bottom: 0;
}
.modalDescription {
  margin-top: 0;
}
.modalLinkButton {
  background-color: rgba(57, 0, 90, 0.5);
}
</style>