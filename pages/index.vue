<template>
  <div class="app">
    <div class="container">
      <img src="@/static/images/logo.svg" alt="logo" />
      <div class="dashboard">
        <div class="selctions">
          <div class="input">
            <label for="bill">Bill</label>
            <div class="holder">
              <input
                @input="checkResetButton"
                type="text"
                id="bill"
                v-model="bill"
                placeholder="0"
              />
              <img src="@/static/images/icon-dollar.svg" alt="" />
            </div>
          </div>
          <div class="tips">
            <span>Select Tip %</span>
            <div class="btns">
              <div @click="chooseTip" data-tip="5" class="btn active">5%</div>
              <div @click="chooseTip" data-tip="10" class="btn">10%</div>
              <div @click="chooseTip" data-tip="15" class="btn">15%</div>
              <div @click="chooseTip" data-tip="25" class="btn">25%</div>
              <div @click="chooseTip" data-tip="50" class="btn">50%</div>
              <input
                v-model="custom"
                @input="customTips"
                type="text"
                class="custom"
                placeholder="Custom"
              />
            </div>
          </div>
          <div class="input">
            <label for="people">Number of People</label>
            <div class="holder">
              <input type="text" id="people" v-model="people" placeholder="0" />
              <img src="@/static/images/icon-person.svg" alt="" />
            </div>
          </div>
        </div>
        <div class="display">
          <div class="tip">
            <div class="description">
              <div class="title">Tip Amount</div>
              <div class="per">/ person</div>
            </div>
            <div class="heading-number">
              ${{ ((bill * (tip / 100)) / people).toFixed(2) }}
            </div>
          </div>
          <div class="total">
            <div class="description">
              <div class="title">Total</div>
              <div class="per">/ person</div>
            </div>
            <div class="heading-number">
              ${{ ((bill * (tip / 100)) / people + bill / people).toFixed(2) }}
            </div>
          </div>
          <div @click="reset" class="reset disabled">reset</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'tipCalculator',
  data() {
    return {
      bill: '',
      people: 1,
      tip: 5,
      custom: '',
    }
  },
  methods: {
    chooseTip(e) {
      const tips = e.target.getAttribute('data-tip')
      this.tip = Number(tips)
      document.querySelectorAll('.btn').forEach((ele) => {
        ele.classList.remove('active')
      })
      e.target.classList.add('active')
      this.custom = ''
    },
    reset() {
      this.bill = 0
      this.people = 1
      this.tip = 0
      document.querySelector('.reset').classList.add('disabled')
      document.querySelectorAll('.btn').forEach((ele) => {
        ele.classList.remove('active')
      })
    },
    customTips() {
      this.tip = Number(this.custom)
      document.querySelectorAll('.btn').forEach((ele) => {
        ele.classList.remove('active')
      })
    },
    checkResetButton() {
      if (this.bill > 0) {
        document.querySelector('.reset').classList.remove('disabled')
      } else {
        document.querySelector('.reset').classList.add('disabled')
      }
    },
  },
}
</script>

<style lang="scss" scoped>
@import '@/assets/variabels';

.app {
  min-height: 100vh;
  background-color: $Lightcyan;
  display: grid;
  place-items: center;
  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    gap: 50px;
    .heading {
      color: $Verydarkcyan;
      font-size: 12px;
      line-height: 1.3;
      letter-spacing: 5px;
      text-transform: uppercase;
    }
    .dashboard {
      background-color: white;
      border-radius: 16px;
      width: 100%;
      padding: 32px;
      display: flex;
      gap: 32px;
      @media (max-width: 767px) {
        flex-direction: column;
      }
      .selctions,
      .display {
        flex: 1;
      }
      .selctions {
        display: flex;
        flex-direction: column;
        gap: 32px;
        .tips {
          span {
            font-weight: 700;
            color: $Darkgrayishcyan;
            margin-bottom: 16px;
            display: inline-block;
          }
          .btns {
            display: flex;
            flex-wrap: wrap;
            gap: 14px;
            .btn {
              background-color: $Verydarkcyan;
              display: flex;
              justify-content: center;
              align-items: center;
              color: white;
              cursor: pointer;
              transition: 0.2s;
              &.active {
                background-color: $Strongcyan;
                color: $Verydarkcyan;
              }
              &:hover {
                background-color: hsl(173, 61%, 77%);
                color: $Verydarkcyan;
              }
            }
            .btn,
            .custom {
              width: calc((100% / 3) - 14px);
              font-size: 24px;
              padding: 6px;
              border-radius: 6px;
              font-weight: 700;
              @media (max-width: 767px) {
                width: calc((100% / 2) - 14px);
              }
            }
            .custom {
              background-color: $Lightgrayishcyan;
              color: $Verydarkcyan;
              text-align: right;
              &::placeholder {
                color: $Verydarkcyan;
                text-align: center;
              }
              &:focus {
                outline: 2px solid $Strongcyan;
              }
            }
          }
        }
      }
      .display {
        background-color: $Verydarkcyan;
        border-radius: 16px;
        padding: 32px;
        display: flex;
        flex-direction: column;
        position: relative;
        gap: 32px;
        .tip,
        .total {
          display: flex;
          justify-content: space-between;
          align-items: center;
          .description {
            .title {
              color: $Lightgrayishcyan;
            }
            .per {
              color: hsl(185deg, 22%, 52%);
              font-weight: 700;
              font-size: 13px;
            }
          }
          .heading-number {
            color: $Strongcyan;
            font-size: 48px;
            font-weight: 700;
            @media (max-width: 767px) {
              font-size: 32px;
            }
          }
        }
        .reset {
          width: calc(100% - 64px);
          text-transform: uppercase;
          position: absolute;
          bottom: 32px;
          left: 32px;
          padding: 8px;
          background-color: $Strongcyan;
          color: $Verydarkcyan;
          font-weight: 700;
          text-align: center;
          font-size: 19px;
          border-radius: 4px;
          cursor: pointer;
          transition: 0.2s;
          @media (max-width: 767px) {
            width: calc(100%);
            position: static;
          }
          &:hover {
            background-color: hsl(173, 61%, 77%);
          }
          &.disabled {
            opacity: 0.2;
            background-color: $Darkgrayishcyan;
            cursor: not-allowed;
          }
        }
      }
    }
  }
}
</style>
