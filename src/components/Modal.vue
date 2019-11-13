<template>
  <!-- This is the modal with the default close button -->
  <div uk-modal>
    <div class="uk-modal-dialog uk-modal-body uk-text-center">
      <button class="uk-modal-close-default" type="button" uk-close></button>
      <h2 class="uk-modal-title">{{novel.Title}}</h2>
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
            <span
              :class="novel.Dropped ? labelDanger : labelSuccess"
            >{{droppedToTextConverter(novel.Dropped)}}</span>
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
        class="cover modalCover"
      />
      <div class="uk-flex uk-flex-around uk-flex-wrap">
        <div>
          <h4>
            Progress:
            <b>{{novel.Progress}}</b>
          </h4>
        </div>
        <div>
          <h4>
            Rank:
            <span class="uk-label tableLabel">{{novel.Rank}}</span>
          </h4>
        </div>
      </div>
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
      labelDanger: "uk-label uk-label-danger tableLabel",
      labelSuccess: "uk-label uk-label-success tableLabel"
    };
  },
  methods: {
    droppedToTextConverter(status) {
      if (status) {
        return "Dropped";
      } else {
        return "Still reading";
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
.modalCover {
  margin-bottom: 1em;
}
.modalDescription {
  margin-top: 0;
}
.modalLinkButton {
  background-color: rgba(57, 0, 90, 0.5);
}
.cover {
  height: 400px;
}
</style>