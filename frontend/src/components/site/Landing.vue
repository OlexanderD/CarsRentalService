<template>
  <div>
    <ClientNavbar class="fadeInDown" v-wow data-wow-delay="0.2s" />
    <p class="display-4 text-center fadeInUp" v-wow data-wow-delay="0.2s">
      Популярний вибір:
    </p>
    <b-card-group deck>
      <div class="d-flex justify-content-center flex-wrap">
        <div v-for="car in popularCars" :key="car.id">
          <div v-if="car.model.imageName != null">
            <b-card
              :img-src="require('@/assets/modelsPhotos/' + car.model.imageName)"
              :img-alt="car.model.model + ' photo'"
              img-top
              class="mb-3 fadeInUp"
              data-wow-delay="0.2s"
              v-wow
            >
              <b-card-title>
                <h3>{{ car.model.brand + " " + car.model.model }}</h3>
              </b-card-title>
              <b-card-text>
                <div class="d-flex justify-content-between align-items-center">
                  <div>
                    <h4>{{ car.model.year }}</h4>
                    <h4>{{ car.model.type }}</h4>
                  </div>
                  <b-link to="/carSelect" class="btn btn-danger">
                    <div class="text-left">Від</div>
                    <div class="d-flex align-items-baseline text-left">
                      <h4 class="m-0">₴{{ car.dayPrice }}</h4>
                      <div class="my-0">/день</div>
                    </div>
                  </b-link>
                </div>
              </b-card-text>
            </b-card>
          </div>
        </div>
      </div>
    </b-card-group>

    <div class="row">
      <div class="col">
        <div class="display-4 text-center fadeInUp" v-wow data-wow-delay="0.2s">
          Доступні для оренди типи авто:
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <b-list-group
          horizontal="lg"
          class="fadeInUp"
          v-wow
          data-wow-delay="0.2s"
        >
          <b-list-group-item
            :to="{ path: '/carSelect', query: { typeName: 'Седан' } }"
            class="d-flex flex-column align-items-center"
          >
            <img src="../../assets/typesIcons/sedan.png" alt="" width="100px" />
            <div class="d-flex justify-content-between align-items-center">
              <h4 class="m-0 mr-2">Седан</h4>
              <b-badge variant="danger">
                <span class="h6">{{ findTypeAmount("Седан") }}</span>
              </b-badge>
            </div>
          </b-list-group-item>
          <b-list-group-item
            :to="{ path: '/carSelect', query: { typeName: 'Купе' } }"
            class="d-flex flex-column align-items-center"
          >
            <img src="../../assets/typesIcons/coupe.png" alt="" width="100px" />
            <div class="d-flex align-items-center">
              <h4 class="m-0 mr-2">Купе</h4>
              <b-badge variant="danger">
                <span class="h6">{{ findTypeAmount("Купе") }}</span>
              </b-badge>
            </div>
          </b-list-group-item>
          <b-list-group-item
            :to="{ path: '/carSelect', query: { typeName: 'Хетчбек' } }"
            class="d-flex flex-column align-items-center"
          >
            <img
              src="../../assets/typesIcons/hatchback.png"
              alt=""
              width="100px"
            />
            <div class="d-flex align-items-center">
              <h4 class="m-0 mr-2">Хетчбек</h4>
              <b-badge variant="danger">
                <span class="h6">{{ findTypeAmount("Хетчбек") }}</span>
              </b-badge>
            </div>
          </b-list-group-item>
          <b-list-group-item
            :to="{ path: '/carSelect', query: { typeName: 'Пікап' } }"
            class="d-flex flex-column align-items-center"
          >
            <img
              src="../../assets/typesIcons/pickup.png"
              alt=""
              width="100px"
            />
            <div class="d-flex align-items-center">
              <h4 class="m-0 mr-2">Пікап</h4>
              <b-badge variant="danger">
                <span class="h6">{{ findTypeAmount("Пікап") }}</span>
              </b-badge>
            </div>
          </b-list-group-item>
          <b-list-group-item
            :to="{
              path: '/carSelect',
              query: { typeName: 'Позашляховик/Кроссовер' },
            }"
            class="d-flex flex-column align-items-center"
          >
            <img src="../../assets/typesIcons/suv.png" alt="" width="100px" />
            <div class="d-flex align-items-center">
              <h4 class="m-0 mr-2">Позашляховик</h4>
              <b-badge variant="danger">
                <span class="h6">{{
                  findTypeAmount("Позашляховик/Кроссовер")
                }}</span>
              </b-badge>
            </div>
          </b-list-group-item>
        </b-list-group>
      </div>
    </div>
    <Footer class="fadeInUp" v-wow data-wow-delay="0.2s" />
  </div>
</template>

<script>
  import DataService from '../../service/DataService'
  import ClientNavbar from './common/ClientNavbar'
  import Footer from './common/Footer'

  export default {
  name: "Landing",
  components: {
    ClientNavbar,
    Footer,
  },
  data() {
    return {
      popularCars: [],
      carsResource: "cars",
      cardGroupKey: 0,
      availableTypes: [],
    };
  },
  methods: {
    loadCars() {
      DataService.retrieveAllRecords(this.carsResource)
        .then((response) => {
          this.findRentableTypes(response);
          this.popularCars = this._.uniqBy(response.data, "model.model").slice(
            0,
            6
          );
        })
        .catch((error) => {
          console.log(error);
        });
    },
    findRentableTypes(response) {
      response.data
        .filter((car) => car.rentable)
        .forEach((car) => {
          const newTypeName = car.model.type;
          const isUpdated = this.availableTypes.find((type) => {
            if (type.name === newTypeName) {
              type.amount += 1;
              return true;
            }
            return false;
          });
          if (!isUpdated) {
            this.availableTypes.push({
              name: newTypeName,
              amount: 1,
            });
          }
        });
    },
    findTypeAmount(typeName) {
      const fountType = this.availableTypes.find((type) => {
        return type.name === typeName;
      });
      return typeof fountType !== "undefined" ? fountType.amount : 0;
    },
  },
  created() {
    this.loadCars();
  },
};
</script>

<style lang="css">
@import "../../styles/main.css";
@import "../../styles/animate.css";

.card {
  max-width: 500px;
}

.features-list > * > li::before {
  content: "";
  display: inline-block;
  width: 30px;
  height: 30px;
  vertical-align: middle;
  background-image: url(https://svgshare.com/i/LKE.svg);
  background-repeat: no-repeat;
  background-size: 100%;
  margin-right: 15px;
}
</style>
