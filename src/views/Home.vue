<template>
  <div>
    <div class="header">
      <h1>SpaceX Launch Programs</h1>
    </div>

    <div class="row">
      <div class="col-3 col-s-12">
        <div class="card">
          <div style="text-align: left; margin-top: 10px; padding-top: 1rem; padding-left: 1rem">
            <b>Filters</b>
          </div>
          <div class="successful">Launch Year</div>
          <hr style="width: 70%" />
          <div class="row myDIV">
            <div id="myDIV" style="text-align: center">
              <div class="col-12 col-s-12">
                <button class="btn btnCorner" :class="{ active: activeBtn === '2006' }" @click="launchYearChange('2006')">2006</button>
                <button class="btn btnCorner" :class="{ active: activeBtn === '2007' }" @click="launchYearChange('2007')">2007</button>
              </div>
              <div class="col-12 col-s-12">
                <button class="btn btnCorner" :class="{ active: activeBtn === '2008' }" @click="launchYearChange('2008')">2008</button>
                <button class="btn btnCorner" :class="{ active: activeBtn === '2009' }" @click="launchYearChange('2009')">2009</button>
              </div>
              <div class="col-12 col-s-12">
                <button class="btn btnCorner" :class="{ active: activeBtn === '2010' }" @click="launchYearChange('2010')">2010</button>
                <button class="btn btnCorner" :class="{ active: activeBtn === '2011' }" @click="launchYearChange('2011')">2011</button>
              </div>
              <div class="col-12 col-s-12">
                <button class="btn btnCorner" :class="{ active: activeBtn === '2012' }" @click="launchYearChange('2012')">2012</button>
                <button class="btn btnCorner" :class="{ active: activeBtn === '2013' }" @click="launchYearChange('2013')">2013</button>
              </div>
              <div class="col-12 col-s-12">
                <button class="btn btnCorner" :class="{ active: activeBtn === '2014' }" @click="launchYearChange('2014')">2014</button>
                <button class="btn btnCorner" :class="{ active: activeBtn === '2015' }" @click="launchYearChange('2015')">2015</button>
              </div>
              <div class="col-12 col-s-12">
                <button class="btn btnCorner" :class="{ active: activeBtn === '2016' }" @click="launchYearChange('2016')">2016</button>
                <button class="btn btnCorner" :class="{ active: activeBtn === '2017' }" @click="launchYearChange('2017')">2017</button>
              </div>
              <div class="col-12 col-s-12">
                <button class="btn btnCorner" :class="{ active: activeBtn === '2018' }" @click="launchYearChange('2018')">2018</button>
                <button class="btn btnCorner" :class="{ active: activeBtn === '2019' }" @click="launchYearChange('2019')">2019</button>
              </div>

              <div class="col-12 col-s-12">
                <button class="btn btnCorner" :class="{ active: activeBtn === '2020' }" @click="launchYearChange('2020')">2020</button>
              </div>
            </div>
          </div>

          <div class="successful">Successful Launch</div>
          <hr style="width: 70%" />
          <div class="row myDIV">
            <div id="myDIV" style="text-align: center">
              <div class="col-12 col-s-12">
                <button class="btn btnCorner" :class="{ active: activeBtnLaunch === 'true' }" @click="launchSuccessLaunch('true')">True</button>
                <button class="btn btnCorner" :class="{ active: activeBtnLaunch === 'false' }" @click="launchSuccessLaunch('false')">False</button>
              </div>
            </div>
          </div>
          <div class="successful">Successful Landing</div>
          <hr style="width: 70%" />
          <div class="row myDIV">
            <div id="myDIV" style="text-align: center">
              <div class="col-12 col-s-12">
                <button class="btn btnCorner" :class="{ active: activeBtnLanding === 'true' }" @click="launchSuccessLanding('true')">True</button>
                <button class="btn btnCorner" :class="{ active: activeBtnLanding === 'false' }" @click="launchSuccessLanding('false')">False</button>
              </div>
            </div>
          </div>
          <div class="card">
            <div class="col-12 col-s-12">
              <button class="btnClear btnCornerClear" @click="Clear">Clear</button>
            </div>
          </div>
        </div>
      </div>
      <div class="col-9 col-s-9">
        <div class="row">
          <div class="col-3 col-s-3" v-for="(item, index) in Data" :key="index">
            <div class="card">
              <img :src="item.links.mission_patch" alt="Missions" width: 100% />
              <div class="container">
                <h4>
                  <b>{{ item.rocket.mission_name }}</b>
                </h4>
                <p>Mission ids:{{ item.mission_id }}</p>
                <p>Launch Year:{{ item.launch_year }}</p>
                <p>Successful Launch:{{ item.launch_success }}</p>
                <p>Successful Landings:</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="footer">
      <p><b>Developed by:</b> Karan Krishna K</p>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  data() {
    return {
      year: null,
      launchsucess: null,
      successlanding: null,
      Data: [],
      activeBtn: "",
      activeBtnLaunch: "",
      activeBtnLanding: "",
    };
  },
  created() {
    this.launches();
  },
  methods: {
    launchYearChange(val) {
      this.activeBtn = val;
      this.launches();
    },
    launchSuccessLaunch(val) {
      this.activeBtnLaunch = val;
      this.launches();
    },
    launchSuccessLanding(val) {
      this.activeBtnLanding = val;
      this.launches();
    },
    Clear() {
      (this.activeBtn = ""), (this.activeBtnLaunch = ""), (this.activeBtnLanding = ""), this.launches();
    },
    async launches() {
      const myHeaders = new Headers();
      myHeaders.append("Content-Type", "application/x-www-form-urlencoded");

      const requestOptions = {
        method: "GET",
      };
      var url = "https://api.spaceXdata.com/v3/launches?limit=100";

      if (this.activeBtn != "") {
        url = url + "&launch_year=" + this.activeBtn;
      }
      if (this.activeBtnLaunch == "true") {
        url = url + "&launch_success=true";
      }
      if (this.activeBtnLaunch == "false") {
        url = url + "&launch_success=false";
      }
      if (this.activeBtnLanding == "true") {
        url = url + "&land_success=true";
      }
      if (this.activeBtnLanding == "false") {
        url = url + "&land_success=false";
      }

      const rawResponse = await fetch(url, requestOptions);

      const response = await rawResponse.json();
      window.scrollTo(0, 0);
      this.Data = response;
      // console.log(response);
    },
  },
};
</script>
<style>
.card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
  width: 100%;
  border-radius: 5px;
  background-color: white;
}

.card:hover {
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
}

img {
  border-radius: 5px 5px 0 0;
}

.container {
  padding: 2px 16px;
}
* {
  box-sizing: border-box;
}

.row::after {
  content: "";
  clear: both;
  display: table;
}

[class*="col-"] {
  float: left;
  padding: 10px;
}

html {
  font-family: "Lucida Sans", sans-serif;
}

.header {
  color: black;
  padding-left: 15px;
}
.btnCorner {
  border-radius: 25px;
  padding: 9px;
  width: 110px;
  height: 33px;
}
.btnCornerClear {
  border-radius: 4px;
  padding: 6px;
  width: 168px;
  height: 33px;
}
.btnClear {
  border: none;
  outline: none;
  margin-left: 10px;
  background-color: #ff6666;
  color: white;
  cursor: pointer;
  font-size: 18px;
}
.btn {
  border: none;
  outline: none;
  margin-left: 10px;
  background-color: #c5df9d;
  cursor: pointer;
  font-size: 18px;
}
.successful {
  text-align: center;
  padding-top: 10px;
  font-weight: bold;
}

/* Style the active class, and buttons on mouse-over */
.active,
.btn:hover {
  background-color: #7cb902;
  color: white;
}
.footer {
  color: black;
  text-align: center;
  font-size: 12px;
  padding: 15px;
}

/* For mobile phones: */
[class*="col-"] {
  width: 100%;
}

@media only screen and (min-width: 700px) {
  /* For tablets: */
  .col-s-1 {
    width: 8.33%;
  }
  .col-s-2 {
    width: 16.66%;
  }
  .col-s-3 {
    width: 25%;
  }
  .col-s-4 {
    width: 33.33%;
  }
  .col-s-5 {
    width: 41.66%;
  }
  .col-s-6 {
    width: 50%;
  }
  .col-s-7 {
    width: 58.33%;
  }
  .col-s-8 {
    width: 66.66%;
  }
  .col-s-9 {
    width: 75%;
  }
  .col-s-10 {
    width: 83.33%;
  }
  .col-s-11 {
    width: 91.66%;
  }
  .col-s-12 {
    width: 100%;
  }
}
@media only screen and (min-width: 1024px) {
  /* For desktop: */
  .col-1 {
    width: 8.33%;
  }
  .col-2 {
    width: 16.66%;
  }
  .col-3 {
    width: 25%;
  }
  .col-4 {
    width: 33.33%;
  }
  .col-5 {
    width: 41.66%;
  }
  .col-6 {
    width: 50%;
  }
  .col-7 {
    width: 58.33%;
  }
  .col-8 {
    width: 66.66%;
  }
  .col-9 {
    width: 75%;
  }
  .col-10 {
    width: 83.33%;
  }
  .col-11 {
    width: 91.66%;
  }
  .col-12 {
    width: 100%;
  }
}
</style>
