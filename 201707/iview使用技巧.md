# Iview使用技巧

## 按i-table某个单元格内容动态生成文字按钮

- 1.使用render函数:

    render:function(h, params) {
                                return h('div', [
                                          h('Button', {
                                              props: {
                                                  type: 'text',
                                                  size: 'small'
                                              },
                                              on: {
                                                  click: function() {
                                                      bill.toOffset(params.index)
                                                  }
                                              }
                                          }, params.row.status)
                                    ]);
                            }

- 2.通过params.row.status来动态指定显示内容