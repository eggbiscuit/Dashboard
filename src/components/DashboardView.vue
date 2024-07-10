<template>
    <div class="dashboard">
      <h1 class="title">帆软软件公司销售驾驶舱</h1>
      <div class="dashboard-content">
        <div class="left-panel">
          <div class="chart-container">
            <canvas ref="regionChart"></canvas>
          </div>
          <div class="chart-container">
            <canvas ref="productLineChart"></canvas>
          </div>
          <div class="chart-container">
            <canvas ref="hotProductChart"></canvas>
          </div>
        </div>
        <div class="center-panel">
          <div class="total-sales">
            <span class="sales-label">全国销售总额</span>
            <span class="sales-value">1234567150</span><span class="currency">元</span>
          </div>
          <div class="map-container">
            <!-- 地图容器 -->
            <div id="chinaMap"></div>
          </div>
        </div>
        <div class="right-panel">
          <div class="chart-container">
            <canvas ref="yearSalesChart"></canvas>
          </div>
          <div class="chart-container">
            <canvas ref="customerChart"></canvas>
          </div>
          <div class="order-info">
            <table>
              <thead>
                <tr>
                  <th>地区</th>
                  <th>渠道</th>
                  <th>成交信息</th>
                  <th>费用</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>北京</td>
                  <td>天猫</td>
                  <td>杨女士购买产品A</td>
                  <td>500</td>
                </tr>
                <tr>
                  <td>上海</td>
                  <td>京东</td>
                  <td>张先生购买产品B</td>
                  <td>1200</td>
                </tr>
                <tr>
                  <td>广州</td>
                  <td>拼多多</td>
                  <td>周女士购买产品C</td>
                  <td>800</td>
                </tr>
                <tr>
                  <td>深圳</td>
                  <td>淘宝</td>
                  <td>你先生购买产品D</td>
                  <td>700</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  import Chart from 'chart.js/auto';
  import * as echarts from 'echarts';
  
  export default {
    name: 'DashboardView',
    setup() {
      const regionChart = ref(null);
      const productLineChart = ref(null);
      const hotProductChart = ref(null);
      const yearSalesChart = ref(null);
      const customerChart = ref(null);
  
      onMounted(() => {
        drawBarChart(regionChart.value, ['广东', '江苏', '浙江', '安徽', '河南'], [1700, 1650, 1600, 1100, 700], '各地区排名');
        drawPieChart(productLineChart.value, [
          { value: 2230, name: '银行理财' },
          { value: 2050, name: '营销保' },
          { value: 2180, name: '股票投顾' },
          { value: 2000, name: '中小保险' }
        ], '产品线占比');
        drawBarChart(hotProductChart.value, ['产品A', '产品B', '产品C', '产品D'], [800, 1500, 1000, 2000], '热门产品');
        drawLineChart(yearSalesChart.value, ['2012', '2013', '2014', '2015'], [800, 1000, 1200, 1500], '历年销售对比');
        drawPieChart(customerChart.value, [
          { value: 33.34, name: '老客户' },
          { value: 66.66, name: '新客户' }
        ], '新老客户占比');
  
        const mapChart = echarts.init(document.getElementById('chinaMap'));
        const mapOption = {
          series: [{
            type: 'map',
            map: 'china'
          }]
        };
        mapChart.setOption(mapOption);
      });
  
      function drawBarChart(canvas, labels, data, label) {
        new Chart(canvas.getContext('2d'), {
          type: 'bar',
          data: {
            labels: labels,
            datasets: [{
              label: label,
              data: data,
              backgroundColor: 'rgba(54, 162, 235, 0.6)',
              borderColor: 'rgba(54, 162, 235, 1)',
              borderWidth: 1
            }]
          },
          options: {
            responsive: true,
            scales: {
              y: {
                beginAtZero: true
              }
            }
          }
        });
      }
  
      function drawPieChart(canvas, data, label) {
        new Chart(canvas.getContext('2d'), {
          type: 'pie',
          data: {
            labels: data.map(item => item.name),
            datasets: [{
              label: label,
              data: data.map(item => item.value),
              backgroundColor: ['rgba(54, 162, 235, 0.6)', 'rgba(75, 192, 192, 0.6)', 'rgba(255, 206, 86, 0.6)', 'rgba(231, 233, 237, 0.6)'],
              borderColor: ['rgba(54, 162, 235, 1)', 'rgba(75, 192, 192, 1)', 'rgba(255, 206, 86, 1)', 'rgba(231, 233, 237, 1)'],
              borderWidth: 1
            }]
          },
          options: {
            responsive: true
          }
        });
      }
  
      function drawLineChart(canvas, labels, data, label) {
        new Chart(canvas.getContext('2d'), {
          type: 'line',
          data: {
            labels: labels,
            datasets: [{
              label: label,
              data: data,
              fill: false,
              borderColor: 'rgb(255, 99, 132)',
              tension: 0.1
            }]
          },
          options: {
            responsive: true,
            scales: {
              y: {
                beginAtZero: true
              }
            }
          }
        });
      }
  
      return {
        regionChart,
        productLineChart,
        hotProductChart,
        yearSalesChart,
        customerChart
      };
    }
  };
  </script>
  
  <style scoped>
  /* .dashboard {
    background: url('../../public/background-image.png') no-repeat center center fixed;
    background-size: cover;
    color: #fff;
    font-family: 'Arial', sans-serif;
  } */
  
  .title {
    text-align: center;
    font-size: 2em;
    margin: 20px 0;
  }
  
  .dashboard-content {
    display: flex;
    justify-content: space-between;
  }
  
  .left-panel,
  .right-panel {
    width: 20%;
  }
  
  .center-panel {
    width: 55%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .chart-container {
    margin-bottom: 20px;
  }
  
  .total-sales {
    display: flex;
    align-items: baseline;
    font-size: 1.5em;
    margin-bottom: 20px;
  }
  
  .sales-label {
    margin-right: 10px;
  }
  
  .sales-value {
    font-size: 2.5em;
    color: gold;
  }
  
  .currency {
    font-size: 1em;
  }
  
  .map-container {
    width: 100%;
    height: 300px;
    margin-bottom: 20px;
  }
  
  .order-info {
    margin-top: 20px;
  }
  
  .order-info table {
    width: 100%;
    border-collapse: collapse;
  }
  
  .order-info th,
  .order-info td {
    border: 1px solid #fff;
    padding: 8px;
    text-align: center;
  }
  
  .order-info th {
    background-color: rgba(0, 0, 0, 0.5);
  }
  </style>
  