<template >
  <div class="hello" >
    <div class="flex">
      <div class="circle" @click.prevent="scrollto('1')" :class="{'circle-active': sel === '1'}"></div>
      <div class="circle" @click.prevent="scrollto('2')" :class="{'circle-active': sel === '2'}"></div>
      <div class="circle" @click.prevent="scrollto('3')" :class="{'circle-active': sel === '3'}"></div>
      <div class="circle" @click.prevent="scrollto('4')" :class="{'circle-active': sel === '4'}"></div>
      <div class="circle" @click.prevent="scrollto('5')" :class="{'circle-active': sel === '5'}"></div>
    </div>
    <div class="full"  :class="{'full-active': sel === '1'}" id="1">1</div>
    <div class="full" :class="{'full-active': sel === '2'}" id="2">2</div>
    <div class="full" :class="{'full-active': sel === '3'}" id="3">3</div>
    <div class="full" :class="{'full-active': sel === '4'}" id="4">4</div>
    <div class="full" :class="{'full-active': sel === '5'}" id="5">5</div>

  </div>

</template>

<script>
export default {
  name: 'HelloWorld',
  data: ()=> {
    return {
      sel: '1',
      scrollPosition: 0,
      pages: document.getElementsByClassName('full'),
      pagesPos: []
    }
  },
  mounted() {
    this.pages.forEach( el => {
      let y = el.getBoundingClientRect().top + window.scrollY;
      this.pagesPos.push(y)
    })

    this.pagesPos.push('5000')
  },
  created () {
    window.addEventListener('scroll', this.onScroll);
  },
  methods : {
    smoothScrollTo(endX, endY, duration) {
      let startX = window.scrollX || window.pageXOffset,
          startY = window.scrollY || window.pageYOffset,
          distanceX = endX - startX,
          distanceY = endY - startY,
          startTime = new Date().getTime();
      let easeInOutQuart = function(time, from, distance, duration) {
        if ((time /= duration / 2) < 1) return distance / 2 * time * time * time * time + from;
        return -distance / 2 * ((time -= 2) * time * time * time - 2) + from;
      };

      let timer = window.setInterval(function() {
        let time = new Date().getTime() - startTime,
            newX = easeInOutQuart(time, startX, distanceX, duration),
            newY = easeInOutQuart(time, startY, distanceY, duration);
        if (time >= duration) {
          window.clearInterval(timer);
        }
        window.scrollTo(newX, newY);
      }, 1000 / 60);
    },

    async onScroll() {
      this.pagesPos.forEach( (el,i) => {
        if (window.scrollY + 50   > el && el < this.pagesPos[i+1]) {
          this.sel = `${i+1}`
        }
      })
    },

    async scrollto(page) {
      this.smoothScrollTo(0,this.pagesPos[page -1], 1000)
    }
  }

}
</script>

<style scoped>

body {
  position: relative;

}
.flex {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: fixed;
  right: 50px;
  top: 50%;
  transform: translate(0,-50%);
}
.circle {
  background: #ffffff;
  cursor: pointer;
  border-radius: 100%;
  border: 1px solid #2c3e50;
  width: 17px;
  height: 17px;
  margin: .7em;
  transition: .2s all, .7s border-radius ease-in-out;
}
.circle:hover {
  background: #e9e6e6;
  transform: scale(1.2);
}
.circle-active {
  background: #282727;
  /*transform: scale(1.2);*/
  border-radius: 1%;
}

.full {
  width: 100vw;
  background: #f1f1f1;
  height: 100vh;
  color: #42b983;
  font-size: 90px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.full-active {
  background: #e7e7e7;
}
</style>
