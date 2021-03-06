<template>
  <div class="song--result__container">
    <router-link :to="getRouterLinkTo"
      ><div
        class="song--result md:my-4 my-3 max-w-3xl mr-auto ml-auto p-2 flex"
      >
        <span
          v-if="isVerifiedMusic"
          title="Indicates music is verified by YouTube"
          class="verified--music py-1 px-2 md:rounded-md md:w-auto md:h-auto w-8 h-8 rounded-full animate-shimmer font-medium"
        >
          <music-icon size="1x" class="md:mr-2 inline music-icon" />
          <span class="md:inline hidden">Verified Music</span>
        </span>
        <div class="song--cover w-1/2">
          <img :src="getCover" alt="" class="rounded-md" loading="lazy" />
        </div>
        <div class="song--details md:px-6 pl-3 md:w-4/6 w-3/4">
          <div class="top--details">
            <h3
              class="md:text-2xl text-md font-semibold title text-gray-600 dark:text-gray-200"
            >
              {{ getTitle }}
            </h3>
          </div>
          <h5
            class="duration md:text-lg text-sm md:mt-2 mt-1 text-gray-700 font-medium dark:text-gray-300"
          >
            {{ getDuration }}
          </h5>
          <h5 class="channel md:text-lg text-sm text-gray-500">
            {{ getChannel }}
          </h5>
          <h5 class="description text-md mt-2 text-gray-500">
            {{ getDescription }}
          </h5>
        </div>
      </div></router-link
    >
  </div>
</template>

<script>
import { MusicIcon } from "vue-feather-icons";

export default {
  name: "SongResult",
  props: {
    song: {
      type: Object
    },
    query: {
      type: String,
      default: ""
    }
  },
  components: {
    MusicIcon
  },
  methods: {
    findCover: function() {
      /**
       * Find the cover of the song.
       *
       * Try to return the highest quality possible. If nothing is available,
       * fallback to a default image.
       */
      const coverObj = this.song.cover;

      if ("large" in coverObj) return coverObj.large;
      else if ("small" in coverObj) return coverObj.large;
      else return "";
    }
  },
  computed: {
    getTitle() {
      return this.song.title;
    },
    getDescription() {
      return this.song.long_desc;
    },
    getChannel() {
      return this.song.channel;
    },
    getDuration() {
      return this.song.duration.readable;
    },
    isVerifiedMusic() {
      return this.song.verified_music;
    },
    getCover() {
      return this.findCover();
    },
    getRouterLinkTo() {
      /**
       * Get the router link to of the current song.
       */
      const videoId = this.song.id;
      return `/metadata?${new URLSearchParams({
        videoId: videoId,
        query: this.query
      }).toString()}`;
    }
  }
};
</script>

<style lang="scss" scoped>
.song--result__container {
  .song--result {
    position: relative;
    .verified--music {
      @extend .dm-sans;
      transition: 0.1s ease;

      background: $teal;
      color: $black;

      position: absolute;
      top: 15px;
      left: 15px;

      .music-icon {
        stroke-width: 3;
        transform: translateY(-1px);
      }
    }

    .song--cover {
      img {
        @media only screen and (min-width: $md) {
          min-width: 20rem;
        }

        @media only screen and (max-width: $md) {
          min-height: 100%;
        }
      }
    }

    .song--details {
      .channel,
      .duration {
        @extend .dm-sans;
      }

      .description,
      .title {
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
        overflow: hidden;
      }

      .description {
        @media only screen and (max-width: $md) {
          display: none;
        }
      }
    }
  }
}
</style>
