<template lang='pug'>
.propertyMap
  .propertyMap__map(ref="map")
  .propertyMap__container
    .propertyMap__data
      .propertyMap__address {{building.info.name}}
      .propertyMap__info
        span.propertyMap__param Area code:
        span {{building.info.areaCode}}
      .propertyMap__info
        span.propertyMap__param Area:
        span {{building.info.area}}
      .propertyMap__info
        span.propertyMap__param Elevation:
        span {{building.info.elevation}}
      .propertyMap__info
        span.propertyMap__param Type:
        span {{building.info.type}}
      BButton.propertyMap__button(color="yellow" rounded @click="openInDemo") More details
</template>

<script>
import {mapGetters} from 'vuex';
import {getBuilding} from 'services/properties';
import {googleMapsKey} from 'config';
import GoogleMapsLoader from 'google-maps';
import Icon from './Icon';
import BButton from './BButton';

export default {
  data() {
    return {
      locationMap: {},
      marker: {},
      building: {
        coordinates: {},
        info: {},
      },
    };
  },
  computed: {
    ...mapGetters('trade', {
      quoteCurrency: 'quoteCurrency',
    }),
  },
  methods: {
    getBuildingData() {
      this.building = getBuilding(this.quoteCurrency);
    },
  },
  watch: {
    quoteCurrency() {
      this.getBuildingData();
      this.marker.setPosition(this.building.coordinates);
      this.locationMap.panTo(this.building.coordinates);
    },
  },
  mounted() {
    this.getBuildingData();
    GoogleMapsLoader.load((google) => {
      this.locationMap = new google.maps.Map(this.$refs.map, {
        zoom: 12,
        center: this.building.coordinates,
        scrollwheel: false,
        fullscreenControl: false,
        zoomControl: false,
        mapTypeControl: false,
        scaleControl: false,
        panControl: false,
        navigationControl: false,
        streetViewControl: false,
        styles: [
          {
            featureType: 'all',
            elementType: 'all',
            stylers: [
              {
                invert_lightness: true,
              },
              {
                saturation: 10,
              },
              {
                lightness: 30,
              },
              {
                gamma: 0.5,
              },
              {
                hue: '#435158',
              },
            ],
          },
        ],
      });
      this.marker = new google.maps.Marker({
        position: this.building.coordinates,
        map: this.locationMap,
        icon: {
          url: require('assets/images/pointer.png'),
          anchor: new google.maps.Point(15, 40),
        },
        animation: google.maps.Animation.DROP,
      });
    });
  },
  created() {
    GoogleMapsLoader.KEY = googleMapsKey;
  },
  components: {
    Icon,
    BButton,
  },
};
</script>

<style lang='scss'>
@import '~variables';
.propertyMap {
  height: 100%;
  position: relative;
  &__map{
    height: 100%;
  }
  &__container {
    width: 100%;
    height: 100%;
    background-color: rgba(3, 53, 79, 0.5);
    position: absolute;
    z-index: 2;
    top: 0;
  }
  &__data {
    margin-top: 5%;
    margin-left: 5%;
    width: 220px;
  }
  &__address {
    font-size: 25px;
    font-weight: 300;
    text-transform: uppercase;
    margin-bottom: 30px;
  }
  &__info {
    display: flex;
    font-size: 12px;
    font-weight: 300;
    margin-bottom: 10px;
  }
  &__param {
    width: 85px;
  }
  &__button {
    margin-top: 35px;
  }
}
</style>
