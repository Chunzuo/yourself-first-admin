<template>
  <div id="dashboard-analytics">
    <div class="vx-row">
      <!-- CARD 1: CONGRATS -->
      <div class="vx-col w-full lg:w-1/2 mb-base">
        <vx-card slot="no-body" class="text-center bg-primary-gradient greet-user">
          <img
            src="@/assets/images/elements/decore-left.png"
            class="decore-left"
            alt="Decore Left"
            width="200"
          />
          <img
            src="@/assets/images/elements/decore-right.png"
            class="decore-right"
            alt="Decore Right"
            width="175"
          />
          <feather-icon
            icon="AwardIcon"
            class="p-6 mb-8 bg-primary inline-flex rounded-full text-white shadow"
            svgClasses="h-8 w-8"
          ></feather-icon>
          <h1 class="mb-6 text-white">Welcome Jillian Holguin,</h1>
          <p class="xl:w-3/4 lg:w-4/5 md:w-2/3 w-4/5 mx-auto text-white">
            <strong>50</strong> users visits
            <strong>YourselfFirst LLC</strong> website today.
          </p>
        </vx-card>
      </div>

      <!-- CARD 2: SUBSCRIBERS GAINED -->
      <div class="vx-col w-full sm:w-1/2 md:w-1/2 lg:w-1/4 xl:w-1/4 mb-base">
        <statistics-card-line
          icon="UsersIcon"
          statistic="92.6k"
          statisticTitle="Subscribers Gained"
          :chartData="subscribers_gained.series"
          type="area"
        ></statistics-card-line>
      </div>

      <!-- CARD 3: ORDER RECIEVED -->
      <div class="vx-col w-full sm:w-1/2 md:w-1/2 lg:w-1/4 xl:w-1/4 mb-base">
        <statistics-card-line
          icon="ShoppingBagIcon"
          statistic="97.5K"
          statisticTitle="Orders Received"
          :chartData="revenueGenerated.series"
          color="warning"
          type="area"
        ></statistics-card-line>
      </div>
    </div>

    <div class="vx-row">
      <!-- CARD 4: SESSION -->
      <div class="vx-col w-full md:w-1/2 mb-base">
        <vx-card>
          <div class="vx-row flex-col-reverse md:flex-col-reverse sm:flex-row lg:flex-row">
            <!-- LEFT COL -->
            <div
              class="vx-col w-full md:w-full sm:w-1/2 lg:w-1/2 xl:w-1/2 flex flex-col justify-between"
              v-if="salesBarSession.analyticsData"
            >
              <div>
                <h2 class="mb-1 font-bold">{{ salesBarSession.analyticsData.session | k_formatter }}</h2>
                <span class="font-medium">Avg Visists</span>

                <!-- Previous Data Comparison -->
                <p class="mt-2 text-xl font-medium">
                  <span
                    :class="salesBarSession.analyticsData.comparison.result >= 0 ? 'text-success' : 'text-danger'"
                  >
                    <span v-if="salesBarSession.analyticsData.comparison.result > 0">+</span>
                    <span>{{ salesBarSession.analyticsData.comparison.result }}</span>
                  </span>
                  <span>vs</span>
                  <span>{{ salesBarSession.analyticsData.comparison.str }}</span>
                </p>
              </div>
              <vs-button
                icon-pack="feather"
                icon="icon-chevrons-right"
                icon-after
                class="shadow-md w-full lg:mt-0 mt-4"
              >View Details</vs-button>
            </div>

            <!-- RIGHT COL -->
            <div
              class="vx-col w-full md:w-full sm:w-1/2 lg:w-1/2 xl:w-1/2 flex flex-col lg:mb-0 md:mb-base sm:mb-0 mb-base"
            >
              <change-time-duration-dropdown class="self-end" />
              <vue-apex-charts
                type="bar"
                height="200"
                :options="salesBar.chartOptions"
                :series="salesBarSession.series"
                v-if="salesBarSession.series"
              />
            </div>
          </div>
          <vs-divider class="my-6"></vs-divider>
          <div class="vx-row">
            <div class="vx-col w-1/2 mb-3">
              <p>Goal: 100</p>
              <vs-progress class="block mt-1" :percent="50" color="primary"></vs-progress>
            </div>
            <div class="vx-col w-1/2 mb-3">
              <p>Users: 50</p>
              <vs-progress class="block mt-1" :percent="60" color="warning"></vs-progress>
            </div>
            <div class="vx-col w-1/2 mb-3">
              <p>Retention: 90%</p>
              <vs-progress class="block mt-1" :percent="70" color="danger"></vs-progress>
            </div>
            <div class="vx-col w-1/2 mb-3">
              <p>Duration: 1yr</p>
              <vs-progress class="block mt-1" :percent="90" color="success"></vs-progress>
            </div>
          </div>
        </vx-card>
      </div>

      <!-- CARD 5: SUPPORT TRACKER -->
      <div class="vx-col w-full md:w-1/2 lg:w-1/2 xl:w-1/2 mb-base">
        <vx-card title="Support Tracker">
          <!-- CARD ACTION -->
          <template slot="actions">
            <change-time-duration-dropdown />
          </template>

          <div slot="no-body" v-if="supportTracker.analyticsData">
            <div class="vx-row text-center">
              <!-- Open Tickets Heading -->
              <div
                class="vx-col w-full lg:w-1/5 md:w-full sm:w-1/5 flex flex-col justify-between mb-4 lg:order-first md:order-last sm:order-first order-last"
              >
                <div class="lg:ml-6 lg:mt-6 md:mt-0 md:ml-0 sm:ml-6 sm:mt-6">
                  <h1 class="font-bold text-5xl">{{ supportTracker.analyticsData.openTickets }}</h1>
                  <small>Tickets</small>
                </div>
              </div>

              <!-- Chart -->
              <div
                class="vx-col w-full lg:w-4/5 md:w-full sm:w-4/5 justify-center mx-auto lg:mt-0 md:mt-6 sm:mt-0 mt-6"
              >
                <vue-apex-charts
                  type="radialBar"
                  height="385"
                  :options="supportTrackerRadialBar.chartOptions"
                  :series="supportTracker.series"
                />
              </div>
            </div>

            <!-- Support Tracker Meta Data -->
            <div class="flex flex-row justify-between px-8 pb-4 mt-4">
              <p
                class="text-center"
                v-for="(val, key) in supportTracker.analyticsData.meta"
                :key="key"
              >
                <span class="block">{{ key }}</span>
                <span class="text-2xl font-semibold">{{ val }}</span>
              </p>
            </div>
          </div>
        </vx-card>
      </div>
    </div>

    <div class="vx-row">
      <div class="vx-col w-full mb-base">
        <vx-card title="Revenue">
          <template slot="actions">
            <feather-icon icon="SettingsIcon" svgClasses="w-6 h-6 text-grey"></feather-icon>
          </template>
          <div slot="no-body" class="p-6 pb-0">
            <div class="flex" v-if="revenueComparisonLine.analyticsData">
              <div class="mr-6">
                <p class="mb-1 font-semibold">This Month</p>
                <p class="text-3xl text-success">
                  <sup class="text-base mr-1">$</sup>
                  {{ revenueComparisonLine.analyticsData.thisMonth.toLocaleString() }}
                </p>
              </div>
              <div>
                <p class="mb-1 font-semibold">Last Month</p>
                <p class="text-3xl">
                  <sup class="text-base mr-1">$</sup>
                  {{ revenueComparisonLine.analyticsData.lastMonth.toLocaleString() }}
                </p>
              </div>
            </div>
            <vue-apex-charts
              type="line"
              height="266"
              :options="revenueComparisonLine.chartOptions"
              :series="revenueComparisonLine.series"
            />
          </div>
        </vx-card>
      </div>
    </div>

    <div class="vx-row">
      <div class="vx-col w-full">
        <vx-card title="Client Retention">
          <div class="flex">
            <span class="flex items-center">
              <div class="h-3 w-3 rounded-full mr-1 bg-primary"></div>
              <span>New Clients</span>
            </span>
            <span class="flex items-center ml-4">
              <div class="h-3 w-3 rounded-full mr-1 bg-danger"></div>
              <span>Retained Clients</span>
            </span>
          </div>
          <vue-apex-charts
            type="bar"
            height="277"
            :options="clientRetentionBar.chartOptions"
            :series="clientRetentionBar.series"
          />
        </vx-card>
      </div>
    </div>
  </div>
</template>

<script>
import StatisticsCardLine from "@/components/statistics-cards/StatisticsCardLine.vue";
import ChangeTimeDurationDropdown from "@/components/ChangeTimeDurationDropdown.vue";

import VueApexCharts from "vue-apexcharts";
export default {
  components: {
    StatisticsCardLine,
    VueApexCharts,
    ChangeTimeDurationDropdown
  },
  data() {
    return {
      subscribers_gained: {
        series: [
          {
            name: "Subscribers",
            data: [28, 40, 36, 52, 38, 60, 55]
          }
        ],
        analyticsData: {
          subscribers: 92600
        }
      },
      revenueGenerated: {
        series: [
          {
            name: "Revenue",
            data: [350, 275, 400, 300, 350, 300, 450]
          }
        ],
        analyticsData: {
          revenue: 97500
        }
      },
      revenueComparisonLine: {
        analyticsData: {
          thisMonth: 86589,
          lastMonth: 73683
        },
        series: [
          {
            name: "This Month",
            data: [45000, 47000, 44800, 47500, 45500, 48000, 46500, 48600]
          },
          {
            name: "Last Month",
            data: [46000, 48000, 45500, 46600, 44500, 46500, 45000, 47000]
          }
        ],
        chartOptions: {
          chart: {
            toolbar: { show: false },
            dropShadow: {
              enabled: true,
              top: 5,
              left: 0,
              blur: 4,
              opacity: 0.1
            }
          },
          stroke: {
            curve: "smooth",
            dashArray: [0, 8],
            width: [4, 2]
          },
          grid: {
            borderColor: "#e7e7e7"
          },
          legend: {
            show: false
          },
          colors: ["#F97794", "#b8c2cc"],
          fill: {
            type: "gradient",
            gradient: {
              shade: "dark",
              inverseColors: false,
              gradientToColors: ["#7367F0", "#b8c2cc"],
              shadeIntensity: 1,
              type: "horizontal",
              opacityFrom: 1,
              opacityTo: 1,
              stops: [0, 100, 100, 100]
            }
          },
          markers: {
            size: 0,
            hover: {
              size: 5
            }
          },
          xaxis: {
            labels: {
              style: {
                cssClass: "text-grey fill-current"
              }
            },
            axisTicks: {
              show: false
            },
            categories: ["01", "05", "09", "13", "17", "21", "26", "31"],
            axisBorder: {
              show: false
            }
          },
          yaxis: {
            tickAmount: 5,
            labels: {
              style: {
                cssClass: "text-grey fill-current"
              },
              formatter: function(val) {
                return val > 999 ? (val / 1000).toFixed(1) + "k" : val;
              }
            }
          },
          tooltip: {
            x: { show: false }
          }
        }
      },
      salesBarSession: {
        series: [
          {
            name: "Sessions",
            data: [75, 125, 225, 175, 125, 75, 25]
          }
        ],
        analyticsData: {
          session: 2700,
          comparison: {
            str: "Last 7 Days",
            result: +5.2
          }
        }
      },
      supportTrackerRadialBar: {
        chartOptions: {
          plotOptions: {
            radialBar: {
              size: 158,
              offsetY: -30,
              startAngle: -150,
              endAngle: 150,
              hollow: {
                size: "65%"
              },
              track: {
                background: "rgba(0,0,0,0)",
                strokeWidth: "100%"
              },
              dataLabels: {
                value: {
                  offsetY: 30,
                  color: "#99a2ac",
                  fontSize: "2rem"
                }
              }
            }
          },
          colors: ["#EA5455"],
          fill: {
            type: "gradient",
            gradient: {
              // enabled: true,
              shade: "dark",
              type: "horizontal",
              shadeIntensity: 0.5,
              gradientToColors: ["#7367F0"],
              inverseColors: true,
              opacityFrom: 1,
              opacityTo: 1,
              stops: [0, 100]
            }
          },
          stroke: {
            dashArray: 8
          },
          chart: {
            sparkline: {}
          },
          labels: ["Completed Tickets"]
        }
      },
      supportTracker: {
        analyticsData: {
          openTickets: 163,
          meta: {
            "New Tickets": 29,
            "Open Tickets": 63,
            "Response Time": "1d"
          }
        },
        series: [83]
      },
      salesBar: {
        // series: [{
        //     name: 'Sessions',
        //     data: [75, 125, 225, 175, 125, 75, 25]
        // }],
        chartOptions: {
          grid: {
            show: false,
            padding: {
              left: 0,
              right: 0
            }
          },
          chart: {
            type: "bar",
            sparkline: { enabled: true },
            toolbar: { show: false }
          },
          states: {
            hover: {
              filter: "none"
            }
          },
          colors: [
            "rgba(115,103,240,0.15)",
            "rgba(115,103,240,0.15)",
            "#7367f0",
            "rgba(115,103,240,0.15)",
            "rgba(115,103,240,0.15)",
            "rgba(115,103,240,0.15)"
          ],
          plotOptions: {
            bar: {
              columnWidth: "45%",
              distributed: true,
              endingShape: "rounded" // Deprecated
              // borderRadius: '20px', // Coming Soon
            }
          },
          tooltip: {
            x: { show: false }
          }
        }
      },
      clientRetentionBar: {
        series: [
          {
            name: "New Clients",
            data: [175, 125, 225, 175, 160, 189, 206, 134, 159, 216, 148, 123]
          },
          {
            name: "Retained Clients",
            data: [
              -144,
              -155,
              -141,
              -167,
              -122,
              -143,
              -158,
              -107,
              -126,
              -131,
              -140,
              -137
            ]
          }
        ],
        chartOptions: {
          grid: {
            borderColor: "#ebebeb",
            padding: {
              left: 0,
              right: 0
            }
          },
          legend: {
            show: false
          },
          dataLabels: {
            enabled: false
          },
          chart: {
            stacked: true,
            type: "bar",
            toolbar: { show: false }
          },
          colors: ["#7367F0", "#EA5455"],
          plotOptions: {
            bar: {
              columnWidth: "10%"
            }
          },
          xaxis: {
            labels: {
              style: {
                cssClass: "text-grey fill-current"
              }
            },
            axisTicks: {
              show: false
            },
            categories: [
              "Jan",
              "Feb",
              "Mar",
              "Apr",
              "May",
              "Jun",
              "Jul",
              "Aug",
              "Sep",
              "Oct",
              "Nov",
              "Dec"
            ],
            axisBorder: {
              show: false
            }
          },
          yaxis: {
            tickAmount: 5,
            labels: {
              style: {
                cssClass: "text-grey fill-current"
              }
            }
          },
          tooltip: {
            x: { show: false }
          }
        }
      }
    };
  }
};
</script>

<style lang="scss">
/*! rtl:begin:ignore */
#dashboard-analytics {
  .greet-user {
    position: relative;

    .decore-left {
      position: absolute;
      left: 0;
      top: 0;
    }
    .decore-right {
      position: absolute;
      right: 0;
      top: 0;
    }
  }

  @media (max-width: 576px) {
    .decore-left,
    .decore-right {
      width: 140px;
    }
  }
}
/*! rtl:end:ignore */
</style>
