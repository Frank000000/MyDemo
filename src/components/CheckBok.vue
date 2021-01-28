<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div id="app">
      <table>
        <!-- 表头 -->
        <tr>
          <th>全选</th>
          <th>
            <input type="checkbox" @click="selectAll($event)" v-model:checked="thChecked" style="zoom:200%">
          </th>
          <!-- <th>地址</th> -->
        </tr>
        <!-- 表身 -->
        <tr :key="index" v-for="(item, index) in tableData">
          <!-- <td>{{item.created_at}}</td> -->
          <td>
            <!-- 在tableDatas数组中查找index，indexOf查找到以后会返回index所在位置，如果tableDatas.indexOf(index)>=0就意味着这个复选框要被勾选 -->
            <input type="checkbox" @click="select(index)" :checked="tableDatas.indexOf(index)>=0" style="zoom:200%">
          </td>
          <td>{{item.name}}</td>

        </tr>
      </table>

      <!-- 用div -->

      <div class="Multistage_check" > 
        多级复选
        <input class="checkbox" type="checkbox" v-model="allCheack" @change="selAll(list)" />
        <span>选择全部</span>
        <div class="row_wrap" >
          <div class="row" v-for="(item,index) in list" :key="index" style="width:200px;height:50px;float:left;display:inline">
            <div class="name">
              {{item.name}}
              <input class="checkbox" type="checkbox" :value="item" v-model="item.IsCheack" @change="selRow1(item)" style="display:inline" />
            </div>
            <div class="row2_wrap">
              <div class="row2" v-for="(item1,index1) in item.list" :key="index1" >
                <div class="name">
                  {{item1.name}}
                  <input class="checkbox" type="checkbox" :value="item1" v-model="item1.IsCheack"
                    @change="selRow2(item, item1)" />
                </div>
                <div class="row3_wrap">
                  <div class="row3" v-for="(item2,index2) in item1.list" :key="index2" style="width:200px;height:50px;float:left;display:inline">
                    <div class="name">
                      {{item2.name}}
                      <input class="checkbox" type="checkbox" :value="item2" v-model="item2.IsCheack"
                        @change="selRow3(item, item1, item2)" />
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="btn" @click="fliterValue()">过滤值</div>
      </div>



    </div>
  </div>
</template>

<script>
  export default {
    name: 'CheckBox',
    data() {
      return {
        msg: '北冥有鱼 其名为鲲 鲲之大 一锅炖不下',
        allCheack: false,
        tableData: [{
          created_at: '2021-1-29 02:00:00',
          description: '张三',
          // id: 'user.user_manage',
          level: '2',
          name: '获取会员信息',
          // parent_id:user.user_manage,
          sort: 1,
          updated_at: '',
          tableData: [{
            created_at: '2021-1-29 02:00:00',
            description: '张三',
            // id: 'user.user_manage',
            level: '2',
            name: '获取会员信息',
            // parent_id:user.user_manage,
            sort: 1,
            updated_at: '',
            tableData: [{
              created_at: '2021-1-29 02:00:00',
              description: '张三',
              // id: 'user.user_manage',
              level: '2',
              name: '获取会员信息',
              // parent_id:user.user_manage,
              sort: 1,
              updated_at: '',
            }, {
              created_at: '2021-1-29 02:00:00',
              description: '张三',
              // id: 'user.user_manage',
              level: '2',
              name: '获取会员信息',
              // parent_id:user.user_manage,
              sort: 1,
              updated_at: '',
            }]
          }, {
            created_at: '2021-1-29 02:00:00',
            description: '张三',
            // id: 'user.user_manage',
            level: '2',
            name: '获取会员信息',
            // parent_id:user.user_manage,
            sort: 1,
            updated_at: '',
          }]
        }, {
          created_at: '2021-1-29 02:00:00',
          description: '张三',
          // id: 'user.user_manage',
          level: '2',
          name: '获取会员信息',
          // parent_id:user.user_manage,
          sort: 1,
          updated_at: ''
        }],
        tableDatas: [], //被选中的行
        thChecked: false, //全选按钮默认为没选中
//---------------------------------------------------------------------------------------------------------------------------
        list: [{
            name: "场地1",
            list: [{
                name: "设备1",
                IsCheack: false,
                list: [{
                    name: "货道1",
                    IsCheack: false
                  },
                  {
                    name: "货道2",
                    IsCheack: false
                  }
                ]
              },
              {
                name: "设备2",
                IsCheack: false,
                list: [{
                    name: "货道1",
                    IsCheack: false
                  },
                  {
                    name: "货道2",
                    IsCheack: false
                  }
                ]
              }
            ]
          },
          {
            name: "场地2",
            list: [{
                name: "设备1",
                IsCheack: false,
                list: [{
                    name: "货道1",
                    IsCheack: false
                  },
                  {
                    name: "货道2",
                    IsCheack: false
                  }
                ]
              },
              {
                name: "设备2",
                IsCheack: false,
                list: [{
                    name: "货道1",
                    IsCheack: false
                  },
                  {
                    name: "货道2",
                    IsCheack: false
                  }
                ]
              }
            ]
          }
        ]

      }
    },
    methods: {
      selectAll($event) {
        if ($event.currentTarget.checked == true) {
          //全选先把数组清空，然后遍历数据，把所有行的index加入到数组中（也就是从0到tableData长度减一）
          this.tableDatas = [];
          for (let i = 0; i < this.tableData.length; i++) {
            this.tableDatas.push(i);
          }
        } else if (($event.currentTarget.checked == false)) {
          this.tableDatas = []; //全不选,则把数组清空
        }
      },
      select(index) {
        let a = this.tableDatas.indexOf(index); //a为当前点击行index在tableDatas中的位置
        if (a >= 0) {
          //存在的话，就从数组中删掉这个
          this.tableDatas.splice(a, 1);
        } else {
          //不存在的话就加入到数组中
          this.tableDatas.push(index);
        }
        //判断全选按钮是否勾选
        if (this.tableDatas.length != this.tableData.length) {
          //数组的长度不等于数据的长度，则全选按钮不勾选
          this.thChecked = false;
        } else {
          //数组的长度等于数据的长度，全选按钮勾选
          this.thChecked = true;
        }
      },

      //另一种 ---------------------------------------------------------------------------------------------
      selAll(list) {
        let that = this;
        if (that.allCheack) {
          list.map((val1, index1) => {
            val1.IsCheack = true;
            val1.list.map((val2, index2) => {
              val2.IsCheack = true;
              val2.list.map((val3, index3) => {
                val3.IsCheack = true;
              });
            });
          });
        } else {
          list.map((val1, index1) => {
            val1.IsCheack = false;
            val1.list.map((val2, index2) => {
              val2.IsCheack = false;
              val2.list.map((val3, index3) => {
                val3.IsCheack = false;
              });
            });
          });
        }
      },

      //场地的选择
      selRow1(item) {
        let that = this;
        /*
        //如果选中
        if (item.IsCheack) {
          //给场地选中
          item.list.map((val1, index1) => {
            val1.IsCheack = true;
            val1.list.map((val2, index2) => {
              val2.IsCheack = true;
            });
          });
          let pan_row1 = that.list.every(function(val, index, arr) {
            return val.IsCheack == true;
          });
          if (pan_row1) {
            that.allCheack = true;
          }
        } else {
          that.allCheack = false;
          item.list.map((val1, index1) => {
            val1.IsCheack = false;
            val1.list.map((val2, index2) => {
              val2.IsCheack = false;
            });
          });
        }
        */

        if (item.IsCheack) {
          //给场地选中
          item.list.map((val1, index1) => {
            val1.IsCheack = true;
            val1.list.map((val2, index2) => {
              val2.IsCheack = true;
            });
          });
        } else {
          item.list.map((val1, index1) => {
            val1.IsCheack = false;
            val1.list.map((val2, index2) => {
              val2.IsCheack = false;
            });
          });
        }

        let pan_row1 = that.list.every(function (val, index, arr) {
          return val.IsCheack == true;
        });
        if (pan_row1) {
          that.allCheack = true;
        } else {
          that.allCheack = false;
        }
      },

      //设备的选择
      selRow2(item, item1) {
        let that = this;
        /*
        //如果选中
        if (item1.IsCheack) {
          item1.list.map((val1, index1) => {
            val1.IsCheack = true;
          });
          let pan_row2 = item.list.every(function(val, index, arr) {
            return val.IsCheack == true;
          });
          if (pan_row2) {
            item.IsCheack = true;
          }
          let pan_row1 = that.list.every(function(val, index, arr) {
            return val.IsCheack == true;
          });
          if (pan_row1) {
            that.allCheack = true;
          }
        } else {
          that.allCheack = false;
          item.IsCheack = false;
          item1.list.map((val1, index1) => {
            val1.IsCheack = false;
          });
        }
        */

        if (item1.IsCheack) {
          item1.list.map((val1, index1) => {
            val1.IsCheack = true;
          });
        } else {
          item1.list.map((val1, index1) => {
            val1.IsCheack = false;
          });
        }

        let pan_row2 = item.list.every(function (val, index, arr) {
          return val.IsCheack == true;
        });
        if (pan_row2) {
          item.IsCheack = true;
        } else {
          item.IsCheack = false;
        }

        let pan_row1 = that.list.every(function (val, index, arr) {
          return val.IsCheack == true;
        });
        if (pan_row1) {
          that.allCheack = true;
        } else {
          that.allCheack = false;
        }
      },

      //货道口的选择
      selRow3(item, item1, item2) {
        let that = this;
        /*
        if (item2.IsCheack) {
          let pan_row3 = item1.list.every(function(val, index, arr) {
            return val.IsCheack == true;
          });
          if (pan_row3) {
            item1.IsCheack = true;
          }
          let pan_row2 = item.list.every(function(val, index, arr) {
            return val.IsCheack == true;
          });
          if (pan_row2) {
            item.IsCheack = true;
          }
          let pan_row1 = that.list.every(function(val, index, arr) {
            return val.IsCheack == true;
          });
          if (pan_row1) {
            that.allCheack = true;
          }
        } else {
          that.allCheack = false;
          item.IsCheack = false;
          item1.IsCheack = false;
        }
        */
        let pan_row3 = item1.list.every(function (val, index, arr) {
          return val.IsCheack == true;
        });
        if (pan_row3) {
          item1.IsCheack = true;
        } else {
          item1.IsCheack = false;
        }

        let pan_row2 = item.list.every(function (val, index, arr) {
          return val.IsCheack == true;
        });
        if (pan_row2) {
          item.IsCheack = true;
        } else {
          item.IsCheack = false;
        }

        let pan_row1 = that.list.every(function (val, index, arr) {
          return val.IsCheack == true;
        });
        if (pan_row1) {
          that.allCheack = true;
        } else {
          that.allCheack = false;
        }
      },

      //过滤数据
      fliterValue() {
        let that = this;
        //拷贝一份数组   避免值互相影响
        let resstr = JSON.stringify(that.list);
        let res = JSON.parse(resstr);

        let row1list = res.filter(function (val, index, arr) {
          let row2list = val.list.filter((val2, index2) => {
            // 过滤货道口
            let row3list = val2.list.filter((val3, index3) => {
              return val3.IsCheack;
            });
            val2.list = row3list;
            //过滤设备
            return row3list.length != 0;
          });

          val.list = row2list;
          //过滤场地
          return row2list.length != 0;
        });

        console.log(row1list);

        that.extractionData(row1list);
      },

      //提取数据
      extractionData(row1list) {
        let resstr = JSON.stringify(row1list);
        let res = JSON.parse(resstr);
        let arr = []; //有几种格式
        // 第一种------ 有选择整个场地时
        // arr = ["场地1", "场地2"];

        // 第二种------ 有选择整个设备时
        // arr = ["场地1", { name: "场地2", list: ["设备1", "设备2"] }];

        // 第三种------ 选择了下面的货道口时
        // arr = [
        //   "场地1",
        //   {
        //     name: "场地2",
        //     list: ["设备1", { name: "设备2", list: ["货道1", "货道2"] }]
        //   }
        // ];

        let arr_str = "";
        res.map((val1, index1) => {
          console.log(val1.IsCheack);
          if (val1.IsCheack) {
            arr.push(val1.name);

            arr_str += val1.name + "、";
          } else {
            let arr1 = [];
            let arr1_str = "";

            val1.list.map((val2, index2) => {
              if (val2.IsCheack) {
                arr1.push(val2.name);

                arr1_str += val2.name + "-";
              } else {
                let arr2 = [];
                let arr2_str = "";

                val2.list.map((val3, index3) => {
                  if (val3.IsCheack) {
                    arr2.push(val3.name);

                    arr2_str += val3.name + ";";
                  }
                });

                arr1.push({
                  name: val2.name,
                  list: arr2
                });

                arr1_str += val2.name + "-" + arr2_str;
              }
            });

            arr.push({
              name: val1.name,
              list: arr1
            });
            arr_str += val1.name + "-" + arr1_str;
          }
        });

        console.log(arr);
        console.log(arr_str);
      }

    }
  }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1,
  h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }

</style>
