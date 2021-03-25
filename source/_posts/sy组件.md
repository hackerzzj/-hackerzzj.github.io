```js
import React, { Component } from 'react'
import ReactDOM from 'react-dom'
import { Toast, WingBlank, WhiteSpace, Icon, Button, NavBar, List, InputItem, Modal, Accordion, Flex, Picker } from 'antd-mobile'
import { observer, inject } from 'mobx-react'
import UserInfo from 'Components/UserInfo/index.js'
import Rules from 'Components/Rules/index.js'
import SYCard from 'Components/SYCard/index'
import jcrdjStore from './jcrdjStore'
import FixedButton from 'Components/FixedButton'
import { createForm } from 'rc-form'
import ImageUploadGroup from 'Components/ImageUploadGroup'
import SYPickerTag from 'Components/SYPickerTag'
import SYSelectTag from 'Components/SYSelectTag'
import MoneyInput from 'Components/MoneyInput'
import { fmoney, rmoney } from 'Util/utils'
import moment from 'moment'
import SyDate from 'Components/SyDate'
import InputValueCheck from 'Common/inputValueCheck'
import Jcrdjmx from 'Components/Jcrdjmx'
import Utils from 'Pages/Utils'
import minus from 'Assets/img/minus-circle.png'
import { Head } from 'Components/PageComponents/mainPageComponents'
import Events from 'Components/Events'
import SYLxSearch from 'Components/SYLxSearch'
import API_PATH from 'Common/apiPath'
import LeftClickIcon from 'Components/LeftClickIcon'
import SYInput from 'Components/SYInput'
import Qyhx from 'Components/Qyhx'
import SyModal from 'Components/SyModal'
import SyUpload from 'Components/SyUpload'
import FkStore from 'Pages/fkStore'
import SYListItem from 'Components/SYListItem'
import SyPopup from '../../../components/SyPopup'
import SYListView from 'Components/SYListView'

let way = ''
let dwzh = ''
let dwbh = ''
let dwmc = ''
let bpmid = ''
const Item = List.Item
const Brief = Item.Brief
const alert = SyPopup.alert

const isIPhone = new RegExp('\\biPhone\\b|\\biPod\\b', 'i').test(window.navigator.userAgent)
let moneyKeyboardWrapProps
if (isIPhone) {
  moneyKeyboardWrapProps = {
    onTouchStart: e => e.preventDefault(),
  }
}

@createForm()
@inject('globalStore')
@observer
class jcrdj extends Component {
  state = {
    gzData: [],
    lrtitle: '',
    sflr: true,
    xggrzh: '',
    jcrlrModel: false,
    sfsfz: true,
    // zjlxmc: '请选择',
    jcrxb: '',
    csny: '请选择',
    csnydate: new Date(),
    jcrqcModel: false,
    jcrmxModel: false,
    jtysr: 0,
    jcrzw: '',
    jcrzy: '',
    jcrzc: '',
    jcrxl: '',
    jcrgddh: '',
    jcrdzyx: '',
    yzbm: '',
    /* jcrzhlistModel: false,
    jcrxm: '',
    jcrzjhm: '',
    jcrzhaddModel: false,*/
    yhsearchModel: false,
    yhaddmc: '',
    yhaddbm: '',
    /* jcrzhmxModel: false,
    khyh: '',
    yhzh: '',
    zhmc: '',
    lrrq: '',*/
    showbt: true,
    zwsearchModel: false,
    zwmc: '请选择',
    zcsearchModel: false,
    zcmc: '请选择',
  }
  showModal = (key) => {
    this.setState({
      [key]: true,
      showbt: false,
    })
  }
  onClose = key => () => {
    this.setState({
      [key]: false,
      showbt: true,
    })
  }
  onClose2 = key => () => {
    this.setState({
      [key]: false
    })
  }
  backClick = () => this.props.history.returnToPortal()
  // 在父组件要求打开本组件中含有SYListView的Modal时, 获取下SYListView应有的高度
  componentDidUpdate() {
    console.log('.................', 'mmmmmmmmmmmmmmmm')
    const { visible } = this.props
    const { ListViewHeight } = this.state
    if (this.SYListView) {
      const correctListViewHeight =
        document.documentElement.clientHeight -
        this.getPositionTop(ReactDOM.findDOMNode(this.SYListView))
      if (ListViewHeight !== correctListViewHeight) {
        this.setState({
          ListViewHeight: correctListViewHeight
        })
        console.log(correctListViewHeight, 'mmmmmmmmmmmmmmmm')
      }
    }
  }  // 获取元素到顶部距离
  getPositionTop(element) {
    let top = 0
    let parents = element.offsetParent
    top += element.offsetTop
    while (parents && !/html|body/i.test(parents.tagName)) {
      top += parents.offsetTop
      parents = parents.offsetParent
    }
    return top
  }
  async componentDidMount() {
    // 给手机公积金传标题名
    if (bbgrxx.blqd === 'app_12329') document.title = '缴存人登记'
    // console.log(this.props.globalStore.basicParam)
    // jcdwpath: this.props.location.state.jumpContent.jcdwpath ? this.props.location.state.jumpContent.jcdwpath : process.env.WAY + '/portal'
    way = this.props.location.state.jumpContent.jcdwpath ? this.props.location.state.jumpContent.jcdwpath : process.env.WAY + '/portal'
    dwzh = this.props.globalStore.basicParam.dwzh ? this.props.globalStore.basicParam.dwzh : ''
    dwbh = this.props.globalStore.basicParam.dwbh ? this.props.globalStore.basicParam.dwbh : ''
    dwmc = this.props.globalStore.basicParam.dwmc ? this.props.globalStore.basicParam.dwmc : ''
    if (dwzh === '') {
      Toast.fail('参数错误！', 3, this.backClick)
    } else {
      Toast.loading('数据加载中……', 0)
      jcrdjStore.getPublicParam()
      await jcrdjStore.getJcrdjbpmid({
        ...jcrdjStore.publicParam,
        dwzh: dwzh
      })
      if (jcrdjStore.jcrdjbpmid.data.bpmid === 0) {
        await jcrdjStore.getJcrdjbpmid2({
          ...jcrdjStore.publicParam,
        })
        bpmid = jcrdjStore.jcrdjbpmid2.results[0].bpmid
      } else {
        bpmid = jcrdjStore.jcrdjbpmid.data.bpmid
      }
      // await jcrdjStore.getJcrdjsqgz({
      //   ...jcrdjStore.publicParam,
      //   ywlx: '0302'
      // })
      // this.setState({ gzData: jcrdjStore.jcrdjsqgz })
      this.rule.getRule({
        ywfl: '01',
        ywlb: '0302',
        dwzh: dwzh,
        objectservice: 'jcdw',
        taskDefinitionName: '发起人',
        lcfx: 1,
        sfzsf: 1,
        zjbzxbm: gjjgrxx.zjbzxbm,
      })
      await jcrdjStore.getJcrdjjcrlist({
        ...jcrdjStore.publicParam,
        bpmid: Number(bpmid),
        page: 1,
        size: 100,
      })
      await jcrdjStore.getJcrdjzjlx({
        zxjgbm: gjjgrxx.zxjgbm,
        tablename: 'bm_zjlx'
      })
      await jcrdjStore.getJcrdjxb({
        zxjgbm: gjjgrxx.zxjgbm,
        tablename: 'bm_xb'
      })
      await jcrdjStore.getJcrdjhyzk({
        zxjgbm: gjjgrxx.zxjgbm,
        tablename: 'bm_hyzk'
      })
      await jcrdjStore.getJcrdjzgzy({
        zxjgbm: gjjgrxx.zxjgbm,
        tablename: 'bm_zgzy'
      })
      await jcrdjStore.getJcrdjzgxl({
        zxjgbm: gjjgrxx.zxjgbm,
        tablename: 'bm_zgxl'
      })
      await jcrdjStore.getJcrdjjcbllist({
        ...jcrdjStore.publicParam,
        dwbh: dwbh,
      })
      if (jcrdjStore.jcrdjjcbllist.length > 0) {
        // 为了传值给SyUpload时方便
        this.setState({ jcblbm: jcrdjStore.jcrdjjcbllist[0].jcblbm })
        Toast.hide()
      } else {
        Toast.fail('单位缴存比例查询失败', 3, this.backClick)
      }
    }
  }
  onDasm = async () => {
    const files = await this.childda.getAllFiles()
    const result = await Utils.uploadEamsImages(files, {
      ...jcrdjStore.publicParam,
      easywfl: '01',
      easywlb: '0302',
      ffbm: '01',
      khbh: dwbh,
      ywlsh: String(bpmid),
      scanpages: 1
    })
    return result
  }
  // 删除档案
  DelDasm = async () => {
    await Utils.getEamsDELData({
      easywfl: '01',
      easywlb: '0302',
      ffbm: '05',
      khbh: dwbh,
      ywlsh: String(bpmid),
    })
  }
  goTjsp = async () => {
    console.log('提交')
    if (jcrdjStore.jcrdjjcrlist.totalcount > 0) {
      Toast.loading('流程运行中……', 0)
      // await jcrdjStore.getJcrdjjygz({
      //   ...jcrdjStore.publicParam,
      //   ywlx: '0302',
      //   ywlsh: String(bpmid),
      // })
      // if (jcrdjStore.jcrdjjygz.success) {
      let fieldsValue = {
        ...jcrdjStore.publicParam,
        bpmid: bpmid,
        lcbz: '0',
        taskId: ' ',
        dwzh: dwzh,
        dwmc: dwmc,
        grbh: bbgrxx.grbh,
        count: jcrdjStore.jcrdjjcrlist.totalcount,
        lcfx: 1,
        sfzsf: 1,
      }
      // await this.onDasm()
      const resultDa = await this.onDasm()
      if (!resultDa.success) {
        message.error('档案上传失败', 3)
        return
      }
      let BsxjyData = await Utils.getBsxjyData({
        easywfl: '01',
        easywlb: '0302',
        ffbm: '06',
        khbh: dwbh,
        ywlsh: String(bpmid),
      })
      if (BsxjyData.ret === 0) {
        await jcrdjStore.getJcrdjjcrsp(fieldsValue)
        if (jcrdjStore.jcrdjjcrsp.success) {
          Toast.success(jcrdjStore.jcrdjjcrsp.msg, 3, this.backClick)
        } else {
          if (jcrdjStore.jcrdjjcrsp.ret === 2) {
            Toast.hide()
            alert({
              content: jcrdjStore.jcrdjjcrsp.msg,
              okText: '继续',
              cancelText: '取消',
              onCancel: async () => {
                Toast.loading('', 0)
                await this.DelDasm()
                Toast.hide()
                this.backClick()
              },
              onOk: async () => {
                Toast.loading('流程运行中...', 0)
                fieldsValue.isSecurity = 1
                await jcrdjStore.getJcrdjjcrsp(fieldsValue)
                if (jcrdjStore.jcrdjjcrsp.success) {
                  Toast.success(jcrdjStore.jcrdjjcrsp.msg, 3, this.backClick)
                } else {
                  this.DelDasm()
                  Toast.fail(jcrdjStore.jcrdjjcrsp.msg, 3)
                }
              }
            })
          } else {
            await this.DelDasm()
            // 查询规则
            this.rule.getRuleById({ sureid: jcrdjStore.jcrdjjcrsp.sureid })
            Toast.fail(jcrdjStore.jcrdjjcrsp.msg, 3)
          }
        }
      } else {
        console.log('必扫项校验失败,正在进行档案删除')
        this.DelDasm()
        Toast.fail(BsxjyData.msg, 3)
      }
      // } else {
      //   this.setState({ gzData: jcrdjStore.jcrdjjygz.data })
      //   Toast.fail('规则校验不通过！', 3)
      // }
    } else {
      Toast.fail('请先录入缴存人！', 3)
    }
  }
  validateJcrdjxx = (rule, value, callback) => {
    const backval = (val) => {
      if (val.success) {
        callback()
      } else {
        callback(val.msg)
      }
    }
    const backerr = (err) => {
      console.log(err)
      callback('校验失败')
    }
    if (rule.fullField === 'jcrzjhm') {
      const zjlx = typeof this.props.form.getFieldValue('jcrzjlx') === 'string' ? this.props.form.getFieldValue('jcrzjlx') : this.props.form.getFieldValue('jcrzjlx')[0]
      if (zjlx === '01') {
        InputValueCheck(value, 'IDCard', '证件号码格式错误').then(backval, backerr)
      } else {
        callback()
      }
    } else if (rule.fullField === 'jcrxm') {
      InputValueCheck(value, 'RealName', '姓名格式错误').then(backval, backerr)
    } else if (rule.fullField === 'jcrsjhm') {
      InputValueCheck(value, 'Mobile', '手机号码格式错误').then(backval, backerr)
    } else if (rule.fullField === 'jcrgddh') {
      if (value.trim() !== '') {
        InputValueCheck(value, 'Phone', '固定电话格式错误').then(backval, backerr)
      } else {
        callback()
      }
    } else if (rule.fullField === 'jcrdzyx') {
      if (value.trim() !== '') {
        InputValueCheck(value, 'Mail', '电子邮箱格式错误').then(backval, backerr)
      } else {
        callback()
      }
    } else if (rule.fullField === 'yzbm') {
      if (value.trim() !== '') {
        InputValueCheck(value, 'Yzbm', '邮政编码格式错误').then(backval, backerr)
      } else {
        callback()
      }
    } else if (rule.fullField === 'jbzhyhzh') {
      InputValueCheck(value, 'BankCard', '存款账户号码格式错误').then(backval, backerr)
    }
  }
  addJcr = async () => {
    await this.props.form.resetFields()
    // console.log('增加缴存人', this.state)
    // console.log(jcrdjStore.jcrdjzjlx.find(e => e.value === '01').label)
    console.log(this.state)
    this.setState({
      grbhchg: '',
      grzhchg: '',
    })
    this.showModal('jcrlrModel')
    /* this.props.form.setFieldsValue({
      jcrzjlx: ['01'],
      jcrzjhm: '',
      jcrsjhm: '',
      jcrhyzk: [],
      jcrjtzz: '',
      grjcjs: 0,
      gryjce: '0.00',
      dwyjce: '0.00',
      jbzhyhzh: '',
      jbzhkhyh: '',
    })*/
    await this.setState({
      // zjlxmc: jcrdjStore.jcrdjzjlx.find(e => e.value === '01').label,
      sflr: true,
      lrtitle: '缴存人登记信息录入',
      sfsfz: true,
      yhaddmc: '',
      yhaddbm: '',
      jtysr: 0,
      jcrzw: '',
      zwmc: '请选择',
      jcrzy: '',
      jcrzc: '',
      zcmc: '请选择',
      jcrxl: '',
      jcrgddh: '',
      jcrdzyx: '',
      yzbm: '',
    })
  }
  changeZjlx = (val) => {
    // this.setState({ zjlxmc: jcrdjStore.jcrdjzjlx.find(e => e.value === val[0]).label })
    if (val[0] === '01') {
      this.setState({ sfsfz: true })
      this.props.form.validateFields((error, value) => {
        if (error == null || error.jcrzjhm === undefined) {
          if (parseInt(value.jcrzjhm.substr(16, 1)) % 2 === 1) {
            this.setState({ jcrxb: '1' })
          } else {
            this.setState({ jcrxb: '2' })
          }
          this.setState({ csny: value.jcrzjhm.substr(6, 4) + '-' + value.jcrzjhm.substr(10, 2) + '-' + value.jcrzjhm.substr(12, 2) })
        }
      })
    } else {
      this.setState({
        sfsfz: false,
        jcrxb: '',
        csny: '请选择',
      })
    }
  }
  xxfx = async() => {
    const { getFieldValue, setFieldsValue, resetFields } = this.props.form
    const zjhm = getFieldValue('jcrzjhm')
    await jcrdjStore.getZgxx({ // 查询bpmid
      'zjhm': zjhm,
    })
    if (jcrdjStore.zgxx.success && jcrdjStore.zgxx.data.length > 0) {
      console.log(jcrdjStore.zgxx.data, '..................')
      const { setFieldsValue } = this.props.form
      const results = jcrdjStore.zgxx.data[0]
      await this.props.form.setFieldsValue({
        jcrxm: results.xingming,
        jcrzjlx: [results.zjlx],
        jcrzjhm: results.zjhm,
        jcrsjhm: results.sjhm,
        jcrhyzk: [results.hyzk],
        jcrjtzz: results.jtzz,
        jbzhyhzh: results.grckzhhm, // 需要一个状态标记是增加还是修改
        jbzhkhyh: results.grckzhkhyhmc,
      })
      if (results.zjlx === '01') {
        this.setState({
          sfsfz: true,
          jcrxb: results.xingbie
        })
      } else {
        this.setState({
          sfsfz: false,
          jcrxb: ''
        })
      }
      this.props.form.setFieldsValue({ jcrxb: [results.xingbie] })
      await this.setState({
        csny: results.csny,
        yhaddmc: results.grckzhkhyhmc,
        yhaddbm: results.grckzhkhyhdm,
        jtysr: results.jtysr,
        jcrzw: results.zhiwu.trim(),
        zwmc: results.zwmc.trim() === '' ? '请选择' : results.zwmc,
        jcrzy: results.zhiye.trim() === '' ? '' : [results.zhiye],
        jcrzc: results.zhichen.trim(),
        zcmc: results.zcmc.trim() === '' ? '请选择' : results.zcmc,
        jcrxl: results.xueli.trim() === '' ? '' : [results.xueli],
        jcrgddh: results.gddhhm.trim(),
        jcrdzyx: results.dzyx.trim(),
        yzbm: results.yzbm.trim(),
      })
      this.setState({ xggrzh: results.grzh, grzhchg: results.grzh, grbhchg: results.grbh })
    } else {
      Toast.fail(jcrdjStore.zgxx.msg)
    }
  }
  // 操作员确认有无风险
  fxtc1 = (msg) => {
    alert({
      content: msg,
      okText: '确定',
      cancelText: '取消',
      onCancel: async () => {
        console.log('Cancel')
      },
      onOk: async () => {
        this.xxfx()
      }
    })
  }
  findcsnyxbByzjhm1 = async() => {
    const { getFieldValue, setFieldsValue, resetFields } = this.props.form
    const zjhm = getFieldValue('jcrzjhm')
    if (window.sfFkPd) {
    // 风控校验一人多户
    // 如果不是修改就走风控
      if (this.state.sflr) {
        await FkStore.getFkpt({
          'zjbzxbm': bbgrxx.zjbzxbm,
          'ywfl': '01',
          'ywlb': '0302',
          'objectservice': 'jcdw',
          'taskDefinitionKey': 'zjhmchg',
          'zxjgbm': gjjgrxx.zxjgbm,
          'zjhm': zjhm,
          'dwzh': dwzh,
        })
        if (FkStore.Fkpt.ret === 1) { // 若存在风险
          Toast.fail(FkStore.Fkpt.yjnr)
          // this.rule.getRuleById({ sureid: FkStore.Fkpt.sureid })
          return
        } else if (FkStore.Fkpt.ret === 2) { // 若存在风险，但不影响业务运行
          this.fxtc1(FkStore.Fkpt.yjnr)
        } else if (FkStore.Fkpt.ret === 0) { // 无风险
          this.findcsnyxbByzjhm()
        }
      } else {
        // 如果是修改，证件号码改变了再走风控
        if (this.state.fkzjhm !== zjhm) {
          await FkStore.getFkpt({
            'zjbzxbm': bbgrxx.zjbzxbm,
            'ywfl': '01',
            'ywlb': '0302',
            'objectservice': 'jcdw',
            'taskDefinitionKey': 'zjhmchg',
            'zxjgbm': gjjgrxx.zxjgbm,
            'zjhm': zjhm,
            'dwzh': dwzh,
          })
          if (FkStore.Fkpt.ret === 1) { // 若存在风险
            Toast.fail(FkStore.Fkpt.yjnr)
            // this.rule.getRuleById({ sureid: FkStore.Fkpt.sureid })
            return
          } else if (FkStore.Fkpt.ret === 2) { // 若存在风险，但不影响业务运行
            this.fxtc1(FkStore.Fkpt.yjnr)
          } else if (FkStore.Fkpt.ret === 0) { // 无风险
            this.findcsnyxbByzjhm()
          }
        } else {
          this.findcsnyxbByzjhm()
        }
      }
    } else {
      this.findcsnyxbByzjhm()
    }
  }
  findcsnyxbByzjhm = () => {
    const zjlx = typeof this.props.form.getFieldValue('jcrzjlx') === 'string' ? this.props.form.getFieldValue('jcrzjlx') : this.props.form.getFieldValue('jcrzjlx')[0]
    if (zjlx === '01') {
      this.props.form.validateFields((error, value) => {
        if (error == null || error.jcrzjhm === undefined) {
          if (parseInt(value.jcrzjhm.substr(16, 1)) % 2 === 1) {
            this.setState({ jcrxb: '1' })
            this.props.form.setFieldsValue({
              jcrxb: ['1']
            })
          } else {
            this.setState({ jcrxb: '2' })
            this.props.form.setFieldsValue({
              jcrxb: ['2']
            })
          }
          this.setState({ csny: value.jcrzjhm.substr(6, 4) + '-' + value.jcrzjhm.substr(10, 2) + '-' + value.jcrzjhm.substr(12, 2) })
        }
      })
    }
  }
  getyjce = async (grjcjs) => {
    await jcrdjStore.getJcrdjyjce({
      ...jcrdjStore.publicParam,
      dwzh: dwzh,
      jcblbm: jcrdjStore.jcrdjjcbllist[0].jcblbm,
      grjcjs: grjcjs
    })
    console.log(jcrdjStore.jcrdjyjce)
    if (jcrdjStore.jcrdjyjce.success) {
      this.props.form.setFieldsValue({
        gryjce: fmoney(jcrdjStore.jcrdjyjce.data.gryjce, 2),
        dwyjce: fmoney(jcrdjStore.jcrdjyjce.data.dwyjce, 2),
      })
    } else {
      this.props.form.setFieldsValue({
        gryjce: '0.00',
        dwyjce: '0.00',
      })
    }
  }
  /* getjcrzhlist = async(xm, zjhm) => {
    await jcrdjStore.getJcrdjjcrzhinfo({
      ...jcrdjStore.publicParam,
      bpmid: bpmid,
      zhmc: xm,
      zjhm: zjhm,
    })
    Toast.hide()
  }
  jcrZhxxlist = async(type, xm, zjhm) => {
    console.log('查看缴存人账户信息')
    Toast.loading('请稍等……', 0)
    if (type === '1') {
      this.props.form.validateFields((error, value) => {
        if (error.jcrxm === undefined && error.jcrzjhm === undefined) {
          this.setState({
            jcrxm: value.jcrxm,
            jcrzjhm: value.jcrzjhm,
          })
          this.getjcrzhlist(value.jcrxm, value.jcrzjhm)
          this.showModal('jcrzhlistModel')
        } else {
          if (error.jcrxm != null) {
            Toast.info(error.jcrxm.errors[0].message, 1)
            return
          }
          if (error.jcrzjhm != null) {
            Toast.info(error.jcrzjhm.errors[0].message, 1)
            return
          }
        }
      })
    } else if (type === '2') {
      this.setState({
        jcrxm: xm,
        jcrzjhm: zjhm,
      })
      this.getjcrzhlist(xm, zjhm)
      this.showModal('jcrzhlistModel')
    }
  }
  addJcrzh = () => {
    console.log('增加个人银行账户')
    this.props.form.setFieldsValue({
      jbzhkhyh: '',
      jbzhyhzh: '',
      jbzhmc: this.state.jcrxm,
    })
    this.setState({
      yhaddmc: '',
      yhaddbm: '',
    })
    this.showModal('jcrzhaddModel')
  }
  saveJcrzhinfo = async(value) => {
    await jcrdjStore.getJcrdjjcrzhadd({
      ...jcrdjStore.publicParam,
      bpmid: bpmid,
      dwzh: dwzh,
      jbzhkhyh: value.jbzhkhyh,
      jbzhyhzh: value.jbzhyhzh,
      jbzhkhyhbm: this.state.yhaddbm,
      zhmc: value.jbzhmc,
      zjhm: this.state.jcrzjhm,
    })
    if (jcrdjStore.jcrdjjcrzhadd.success) {
      this.getjcrzhlist(this.state.jcrxm, this.state.jcrzjhm)
      this.setState({ jcrzhaddModel: false })
    } else {
      Toast.fail(jcrdjStore.jcrdjjcrzhadd.msg, 2)
    }
  }
  saveJcrzh = () => {
    console.log('保存个人银行账户')
    Toast.loading('请稍等……', 0)
    this.props.form.validateFields((error, value) => {
      if (error.jbzhkhyh === undefined && error.jbzhyhzh === undefined && error.jbzhmc === undefined) {
        this.saveJcrzhinfo(value)
      } else {
        if (error.jbzhkhyh != null) {
          Toast.info(error.jbzhkhyh.errors[0].message, 1)
          return
        }
        if (error.jbzhyhzh != null) {
          Toast.info(error.jbzhyhzh.errors[0].message, 1)
          return
        }
        if (error.jbzhmc != null) {
          Toast.info(error.jbzhmc.errors[0].message, 1)
          return
        }
      }
    })
  }*/
  handleYhsearch = (v) => {
    console.log(v)
    if (v) {
      this.setState({
        yhsearchModel: false,
        yhaddmc: v.mc,
        yhaddbm: v.bm,
      })
      this.props.form.setFieldsValue({ jbzhkhyh: v.mc })
    }
  }
  zwSearch = (v) => {
    if (v) {
      this.setState({
        zwsearchModel: false,
        zwmc: v.mc,
      })
      this.props.form.setFieldsValue({ jcrzw: v.bm })
    }
  }
  zcSearch = (v) => {
    if (v) {
      this.setState({
        zcsearchModel: false,
        zcmc: v.mc,
      })
      this.props.form.setFieldsValue({ jcrzc: v.bm })
    }
  }
  /* jcrzhxx = (khyh, yhzh, zhmc, lrrq) => {
    this.setState({
      khyh: khyh,
      yhzh: yhzh,
      zhmc: zhmc,
      lrrq: lrrq,
    })
    this.showModal('jcrzhmxModel')
  }
  deljcrzhinfo = async(zhmc, yhzh) => {
    Toast.loading('请稍等……', 0)
    await jcrdjStore.getJcrdjjcrzhdel({
      ...jcrdjStore.publicParam,
      bpmid: bpmid,
      dwzh: dwzh,
      zhmc: zhmc,
      jbzhyhzh: yhzh,
    })
    if (jcrdjStore.jcrdjjcrzhdel.success) {
      this.getjcrzhlist(this.state.jcrxm, this.state.jcrzjhm)
      this.setState({ jcrzhmxModel: false })
    } else {
      Toast.fail(jcrdjStore.jcrdjjcrzhdel.msg, 2)
    }
  }
  delJcrzh = (zhmc, yhzh) => {
    console.log('要删除的银行账户：' + yhzh)
    alert('删除', '删除个人账户？', [
      { text: '取消', onPress: () => {
        console.log('取消删除银行账户：' + yhzh)
      } },
      { text: '确定', onPress: () => {
        console.log('删除银行账户：' + yhzh)
        this.deljcrzhinfo(zhmc, yhzh)
      } }
    ])
  }*/
  // 操作员确认有无风险
  fxtc = (msg,value, jcrxb, jcrcsny) => {
    Modal.confirm({
      centered: true,
      title: '提示',
      content: msg,
      onOk: async () => {
        this.saveJcrinfo(value, jcrxb, jcrcsny)
      },
      onCancel: async () => {
      },
    })
  }
  saveJcrinfo2 = async (value, jcrxb, jcrcsny) => {
    if (window.sfFkPd) {
      Toast.loading('校验中...', 0)
      await FkStore.getFkpt({
        'zjbzxbm': bbgrxx.zjbzxbm,
        'ywfl': '01',
        'ywlb': '0302',
        'jgbm': gjjgrxx.jgbm,
        'objectservice': 'jcdw',
        'taskDefinitionKey': 'add',
        'zxjgbm': gjjgrxx.zxjgbm,
        'xingming': value.jcrxm,
        'zjhm': value.jcrzjhm,
        'grckzhhm': value.jbzhyhzh,
        'dwzh': dwzh,
        'grjcjs': value.grjcjs,
        'gryjce': rmoney(value.gryjce),
        'dwyjce': rmoney(value.dwyjce),
        'sjhm': value.jcrsjhm.replace(/\s*/g, ''),
        'grzh': this.state.xggrzh,
        'csrq': jcrcsny
      })
      if (FkStore.Fkpt.ret === 1) {
        Toast.fail(FkStore.Fkpt.yjnr,3)
        return
      } else if (FkStore.Fkpt.ret === 2) {
        Toast.hide()
        this.fxtc(FkStore.Fkpt.yjnr, value, jcrxb, jcrcsny)
      } else {
        Toast.success('校验成功',1)
        this.saveJcrinfo(value, jcrxb, jcrcsny)
      }
    } else {
      this.saveJcrinfo(value, jcrxb, jcrcsny)
    }
  }
  saveJcrinfo = async (value, jcrxb, jcrcsny) => {
    if (!this.state.sflr) {
      if (!window.sfFkPd) {
        await jcrdjStore.getJcrdjjcrdel({
          ...jcrdjStore.publicParam,
          end: 1,
          grzh: this.state.xggrzh,
          dwzh: dwzh,
        })
        if (!jcrdjStore.jcrdjjcrdel.success) {
          Toast.fail('缴存人修改失败', 3)
          return
        }
      }
    }
    await jcrdjStore.getJcrdjjcradd({
      ...jcrdjStore.publicParam,
      bpmid: bpmid,
      dwzh: dwzh,
      grbh: this.state.grbhchg,
      xingming: value.jcrxm,
      zjlx: typeof value.jcrzjlx === 'string' ? value.jcrzjlx : value.jcrzjlx[0],
      zjhm: value.jcrzjhm,
      xingbie: jcrxb,
      csny: jcrcsny,
      grlbbm: '01',
      gddhhm: value.jcrgddh.trim() === '' ? ' ' : value.jcrgddh,
      sjhm: value.jcrsjhm.replace(/\s*/g, ''),
      yzbm: value.yzbm.trim() === '' ? ' ' : value.yzbm,
      zhiwu: value.jcrzw === '' ? ' ' : value.jcrzw,
      zhiye: value.jcrzy === '' ? ' ' : value.jcrzy[0],
      zhichen: value.jcrzc === '' ? ' ' : value.jcrzc,
      hyzk: typeof value.jcrhyzk === 'string' ? value.jcrhyzk : value.jcrhyzk[0],
      jtzz: value.jcrjtzz,
      jtysr: value.jtysr === '' ? ' ' : value.jtysr,
      xueli: value.jcrxl === '' ? ' ' : value.jcrxl[0],
      grjcjs: value.grjcjs,
      jcblbm: jcrdjStore.jcrdjjcbllist[0].jcblbm,
      gryjce: rmoney(value.gryjce),
      dwyjce: rmoney(value.dwyjce),
      dzyx: value.jcrdzyx.trim() === '' ? ' ' : value.jcrdzyx,
      grckzhkhyhmc: value.jbzhkhyh,
      grckzhhm: value.jbzhyhzh,
      grckzhkhyhdm: this.state.yhaddbm,
      czan: this.state.sflr ? '1' : '2',
      grzh: this.state.grzhchg
    })
    await jcrdjStore.getJcrdjjcrlist({
      ...jcrdjStore.publicParam,
      bpmid: Number(bpmid),
      page: 1,
      size: 100,
    })
    if (jcrdjStore.jcrdjjcradd.success) {
      Toast.success('缴存人保存成功', 3)
      this.setState({
        csny: '请选择',
        grbhchg: '',
        grzhchg: '',
      })
      if (this.SYListView !== null && this.SYListView !== undefined) {
        this.SYListView.onRefresh()
      }
      if (this.state.sflr) {
        this.setState({ jcrlrModel: false, showbt: true })
      } else {
        this.setState({ jcrlrModel: false })
      }
    } else {
      Toast.fail(jcrdjStore.jcrdjjcradd.msg, 3)
      if (!this.state.sflr) {
        // this.setState({ jcrlrModel: false, jcrqcModel: false, showbt: true })
        this.setState({ jcrqcModel: false, sflr: true })
      }
    }
  }
  saveJcr = () => {
    console.log('保存缴存人')
    Toast.loading('请稍等……', 0)
    this.props.form.validateFields((error, value) => {
      /* if (error.jcrxm === undefined && error.jcrzjlx === undefined && error.jcrzjhm === undefined && error.jcrsjhm === undefined && error.jcrhyzk === undefined && error.jcrjtzz === undefined && error.grjcjs === undefined && error.gryjce === undefined && error.dwyjce === undefined && error.jtysr === undefined && error.jcrzw === undefined && error.jcrzy === undefined && error.jcrzc === undefined && error.jcrxl === undefined && error.jcrgddh === undefined && error.jcrdzyx === undefined && error.yzbm === undefined) {
        let jcrcsny = this.state.csny
        let jcrxb = ''
        if (jcrcsny === '' || jcrcsny === '请选择') {
          Toast.info('请选择出生年月', 1)
          return
        } else {
          const zjlx = typeof this.props.form.getFieldValue('jcrzjlx') === 'string' ? this.props.form.getFieldValue('jcrzjlx') : this.props.form.getFieldValue('jcrzjlx')[0]
          if (zjlx === '01') {
            jcrxb = this.state.jcrxb
          } else {
            if (error.jcrxb === undefined) {
              jcrxb = value.jcrxb
            } else {
              if (error.jcrxb != null) {
                Toast.info(error.jcrxb.errors[0].message, 1)
                return
              }
            }
          }
          console.log(value, jcrxb, jcrcsny)// 增加缴存人接口
          this.saveJcrinfo(value, jcrxb, jcrcsny)
        }
      }*/
      if (error === null) {
        let jcrcsny = this.state.csny
        let jcrxb = ''
        if (jcrcsny === '' || jcrcsny === '请选择') {
          Toast.fail('请选择出生年月', 3)
          return
        } else {
          const zjlx = typeof this.props.form.getFieldValue('jcrzjlx') === 'string' ? this.props.form.getFieldValue('jcrzjlx') : this.props.form.getFieldValue('jcrzjlx')[0]
          if (zjlx === '01') {
            jcrxb = this.state.jcrxb
          } else {
            jcrxb = typeof value.jcrxb === 'string' ? value.jcrxb : value.jcrxb[0]
          }
          if (value.grjcjs <= 100000 && value.grjcjs > 0) {
            console.log(value, jcrxb, jcrcsny)
            // Toast.loading('校验中...', 0)
            /* await FkStore.getFkpt({
              'zjbzxbm': 'yun4dx',
              'ywfl': '01',
              'ywlb': '0302',
              'objectservice': 'jcdw',
              'grckzhhm': value.jbzhyhzh,
              'zxjgbm': gjjgrxx.zxjgbm,
              'zjhm': value.jcrzjhm,
              'dwzh': dwzh,
              'sjhm': value.jcrsjhm
            })
            if (FkStore.Fkpt.ret === 1) {
              Toast.fail(FkStore.Fkpt.yjnr,3)
              return
            } else if (FkStore.Fkpt.ret === 2) {
              this.fxtc(FkStore.Fkpt.yjnr,value,jcrxb,jcrcsny)
            } else {
              this.saveJcrinfo(value, jcrxb, jcrcsny)
            }*/
            this.saveJcrinfo2(value, jcrxb, jcrcsny)
          } else {
            Toast.fail('缴存基数必须大于等于1、小于等于100,000', 3)
          }
        }
      } else {
        if (error.jcrxm != null) {
          Toast.fail(error.jcrxm.errors[0].message, 3)
          return
        }
        if (error.jcrzjlx != null) {
          Toast.fail(error.jcrzjlx.errors[0].message, 3)
          return
        }
        if (error.jcrzjhm != null) {
          Toast.fail(error.jcrzjhm.errors[0].message, 3)
          return
        }
        if (error.jcrxb != null) {
          Toast.fail(error.jcrxb.errors[0].message, 3)
          return
        }
        if (error.jcrsjhm != null) {
          Toast.fail(error.jcrsjhm.errors[0].message, 3)
          return
        }
        if (error.jcrhyzk != null) {
          Toast.fail(error.jcrhyzk.errors[0].message, 3)
          return
        }
        if (error.jcrjtzz != null) {
          Toast.fail(error.jcrjtzz.errors[0].message, 3)
          return
        }
        if (error.grjcjs != null) {
          Toast.fail(error.grjcjs.errors[0].message, 3)
          return
        }
        if (error.gryjce != null) {
          Toast.fail(error.gryjce.errors[0].message, 3)
          return
        }
        if (error.dwyjce != null) {
          Toast.fail(error.dwyjce.errors[0].message, 3)
          return
        }
        if (error.jtysr != null) {
          Toast.fail(error.jtysr.errors[0].message, 3)
          return
        }
        if (error.jcrzw != null) {
          Toast.fail(error.jcrzw.errors[0].message, 3)
          return
        }
        if (error.jcrzy != null) {
          Toast.fail(error.jcrzy.errors[0].message, 3)
          return
        }
        if (error.jcrzc != null) {
          Toast.fail(error.jcrzc.errors[0].message, 3)
          return
        }
        if (error.jcrxl != null) {
          Toast.fail(error.jcrxl.errors[0].message, 3)
          return
        }
        if (error.jcrgddh != null) {
          Toast.fail(error.jcrgddh.errors[0].message, 3)
          return
        }
        if (error.jcrdzyx != null) {
          Toast.fail(error.jcrdzyx.errors[0].message, 3)
          return
        }
        if (error.yzbm != null) {
          Toast.fail(error.yzbm.errors[0].message, 3)
          return
        }
        if (error.jbzhyhzh != null) {
          Toast.fail(error.jbzhyhzh.errors[0].message, 3)
          return
        }
        if (error.jbzhkhyh != null) {
          Toast.fail(error.jbzhkhyh.errors[0].message, 3)
          return
        }
      }
    })
  }
  // 操作员确认有无风险
  /* fxtc = (msg,value,jcrxb,jcrcsny) => {
    Modal.confirm({
      centered: true,
      title: '提示',
      content: msg,
      onOk: async () => {
        this.saveJcrinfo(value, jcrxb, jcrcsny)
      },
      onCancel: async () => {
      },
    })
  }*/
  openJcrlist = () => {
    console.log('打开缴存人登记清册')
    const jcrs = jcrdjStore.jcrdjjcrlist.totalcount ? jcrdjStore.jcrdjjcrlist.totalcount : 0
    if (jcrs > 0) {
      this.showModal('jcrqcModel')
    }
  }
  jcrmxinfo = async (grzh) => {
    console.log('要查看的缴存人信息grzh：' + grzh)
    Toast.loading('请稍等……', 0)
    await this.props.form.resetFields()
    /* await jcrdjStore.getJcrdjjcrinfo({
      ...jcrdjStore.publicParam,
      grbh: grbh
    })
    Toast.hide()
    this.showModal('jcrmxModel')*/
    await jcrdjStore.getJcrdjjcrxginfo({
      ...jcrdjStore.publicParam,
      grzh: grzh,
      dwzh: dwzh
    })
    Toast.hide()
    if (jcrdjStore.jcrdjjcrxginfo) {
      // this.showModal('jcrlrModel')
      this.setState({ jcrlrModel: true })
      await this.props.form.setFieldsValue({
        jcrxm: jcrdjStore.jcrdjjcrxginfo.xingming,
        jcrzjlx: [jcrdjStore.jcrdjjcrxginfo.zjlx],
        jcrzjhm: jcrdjStore.jcrdjjcrxginfo.zjhm,
        jcrsjhm: jcrdjStore.jcrdjjcrxginfo.sjhm,
        jcrhyzk: [jcrdjStore.jcrdjjcrxginfo.hyzk],
        jcrjtzz: jcrdjStore.jcrdjjcrxginfo.jtzz,
        grjcjs: jcrdjStore.jcrdjjcrxginfo.grjcjs,
        gryjce: fmoney(jcrdjStore.jcrdjjcrxginfo.gryjce, 2),
        dwyjce: fmoney(jcrdjStore.jcrdjjcrxginfo.dwyjce, 2),
        jbzhyhzh: jcrdjStore.jcrdjjcrxginfo.grckzhhm, // 需要一个状态标记是增加还是修改
        jbzhkhyh: jcrdjStore.jcrdjjcrxginfo.grckzhkhyhmc,
      })
      if (jcrdjStore.jcrdjjcrxginfo.zjlx === '01') {
        this.setState({
          sfsfz: true,
          jcrxb: jcrdjStore.jcrdjjcrxginfo.xingbie
        })
      } else {
        this.setState({
          sfsfz: false,
          jcrxb: ''
        })
      }
      this.props.form.setFieldsValue({ jcrxb: [jcrdjStore.jcrdjjcrxginfo.xingbie] })
      await this.setState({
        sflr: false,
        lrtitle: '缴存人登记信息修改',
        xggrzh: grzh,
        csny: jcrdjStore.jcrdjjcrxginfo.csny,
        yhaddmc: jcrdjStore.jcrdjjcrxginfo.grckzhkhyhmc,
        yhaddbm: jcrdjStore.jcrdjjcrxginfo.grckzhkhyhdm,
        jtysr: jcrdjStore.jcrdjjcrxginfo.jtysr,
        jcrzw: jcrdjStore.jcrdjjcrxginfo.zhiwu.trim(),
        zwmc: jcrdjStore.jcrdjjcrxginfo.zhiwumc.trim() === '' ? '请选择' : jcrdjStore.jcrdjjcrxginfo.zhiwumc,
        jcrzy: jcrdjStore.jcrdjjcrxginfo.zhiye.trim() === '' ? '' : [jcrdjStore.jcrdjjcrxginfo.zhiye],
        jcrzc: jcrdjStore.jcrdjjcrxginfo.zhichen.trim(),
        zcmc: jcrdjStore.jcrdjjcrxginfo.zhichengmc.trim() === '' ? '请选择' : jcrdjStore.jcrdjjcrxginfo.zhichengmc,
        jcrxl: jcrdjStore.jcrdjjcrxginfo.xueli.trim() === '' ? '' : [jcrdjStore.jcrdjjcrxginfo.xueli],
        jcrgddh: jcrdjStore.jcrdjjcrxginfo.gddhhm.trim(),
        jcrdzyx: jcrdjStore.jcrdjjcrxginfo.dzyx.trim(),
        yzbm: jcrdjStore.jcrdjjcrxginfo.yzbm.trim(),
        grbhchg: jcrdjStore.jcrdjjcrxginfo.grbh,
        grzhchg: jcrdjStore.jcrdjjcrxginfo.grzh,
      })
    } else {
      Toast.fail('缴存人详细信息查询失败', 3)
    }
  }
  deljcrinfo = async (grzh) => {
    Toast.loading('请稍等……', 0)
    await jcrdjStore.getJcrdjjcrdel({
      ...jcrdjStore.publicParam,
      end: 1,
      grzh: grzh,
      dwzh: dwzh,
    })
    if (jcrdjStore.jcrdjjcrdel.success) {
      Toast.success('缴存人删除成功', 1)
      this.setState({ jcrmxModel: false })
      await jcrdjStore.getJcrdjjcrlist({
        ...jcrdjStore.publicParam,
        bpmid: bpmid,
        page: 1,
        size: 100,
      })
      this.SYListView.onRefresh()
      const jcrs = jcrdjStore.jcrdjjcrlist.totalcount ? jcrdjStore.jcrdjjcrlist.totalcount : 0
      if (jcrs === 0) {
        this.setState({ jcrqcModel: false, showbt: true })
      }
    } else {
      Toast.fail(jcrdjStore.jcrdjjcrdel.msg, 3)
    }
  }
  delJcr = async (grzh) => {
    console.log('要删除的个人账号：' + grzh)
    this.deleteAlert = alert({
      content: '您确定删除这个缴存人吗？',
      okText: '确定',
      cancelText: '取消',
      onCancel: () => { console.log('cancel') },
      onOk: async () => {
        // this.deleteAlert.hidden = true
        console.log('已删除id：' + grzh)
        this.deljcrinfo(grzh)
      }
    })
  }
  changeDate = (type, date) => {
    let dateStr = moment(date).format('YYYY-MM-DD')
    console.log('选择date==>', dateStr, date)
    this.setState({
      [type]: dateStr,
      [type + 'date']: date
    })
  }
  closeLr = () => {
    if (this.state.sflr) {
      this.setState({ jcrlrModel: false, showbt: true, csny: '请选择' })
    } else {
      this.setState({ jcrlrModel: false, csny: '请选择' })
    }
    this.setState({
      grbhchg: '',
      grzhchg: '',
    })
  }
  // 打开企业画像
  opQyhx = async () => {
    console.log(this.props.globalStore.basicParam)
    let params = {
      ...gjjgrxx,
      dwbh: this.props.globalStore.basicParam.dwbh,
      grbh: this.props.globalStore.basicParam.dwbh,
      zzjgdm: this.props.globalStore.basicParam.zzjgdm,
      userid: bbgrxx.userid,
      zxjgbm: gjjgrxx.zxbm,
      cplx: '2'
    }
    const urldata = Utils.getUrl(params, '缴存单位')
    await urldata.then((v) => {
      this.setState({
        urldata: v
      })
    })
    if (this.state.urldata === '') {
      Toast.info('暂无画像', 3)
      return
    }
    this.setState({
      qyhx: true,
      showbt: false,
    })
  }
  // 关闭企业画像
  closeQyhx = () => {
    this.setState({
      qyhx: false,
      showbt: true,
    })
  }

  render() {
    const { getFieldProps } = this.props.form
    const columns = [
      {
        title: '证件号码',
        key: 'zjhm',
      },
      {
        title: '个人缴存基数',
        key: 'grjcjs',
        render: (item) => {
          return fmoney(item, 2)
        }
      }
    ]
    const renderRow = (rowData, sectionID, rowID, showDetail) => {
      return (
        <SYListItem
          title={rowData.xingming}
          rowData={rowData}
          columns={columns}
          style={{ marginTop: '2.875rem' }}
          buttonList={[
            {
              text: '修改',
              type: 0,
              onPress: () => this.jcrmxinfo(rowData.grzh)
            },
            {
              text: '删除',
              type: 1,
              onPress: () => this.delJcr(rowData.grzh)
            }
          ]}
          rowNum={2}
        />
      )
    }
    return (
      <div>
        <SyModal
          visible={this.state.jcrlrModel}
          ref={ref => (this.jcrlrModel = ref)}
          animationType='slide-up'
          childModals={[this.zcsearchModel, this.zwsearchModel, this.yhsearchModel]}
          changeStateTag={this.closeLr}
        >
          <div style={{ height: '100vh' }}>
            {bbgrxx && bbgrxx.blqd !== 'app_12329' ? <NavBar
              mode='light'
              icon={<LeftClickIcon />}
              onLeftClick={this.closeLr}
            >{this.state.lrtitle}</NavBar> : <NavBar
              mode='light'
            >{this.state.lrtitle}</NavBar>}
            <SYCard showWhiteSpace={false} full>
              <SYCard.Body>
                <List style={{ paddingTop: '2.875rem' }} className='WPK-noBodyLine sylist'>
                  <InputItem
                    type='text'
                    placeholder='请输入'
                    clear
                    maxLength='25'
                    {...getFieldProps('jcrxm', {
                      rules: [{
                        required: true, message: '请输入姓名'
                      }, {
                        validator: this.validateJcrdjxx
                      }]
                    })}
                  >姓名</InputItem>
                  <SYPickerTag
                    dataSource={jcrdjStore.jcrdjzjlx}
                    onChangeGetSelet={(v) => { this.changeZjlx(v) }}
                    // extra={this.state.zjlxmc}
                    {...getFieldProps('jcrzjlx', {
                      initialValue: '01',
                      rules: [{ required: true, message: '请选择证件类型' }]
                    })}
                  >证件类型</SYPickerTag>
                  <InputItem
                    type='text'
                    style={{ textAlign: 'right' }}
                    placeholder='请输入'
                    clear
                    maxLength={18}
                    onBlur={() => { this.findcsnyxbByzjhm1() }}
                    {...getFieldProps('jcrzjhm', {
                      rules: [{
                        required: true, message: '请输入证件号码'
                      }, {
                        validator: this.validateJcrdjxx
                      }]
                    })}
                  >证件号码</InputItem>
                  <List className='WPK-noBodyLine sylist'>
                    <Item
                      arrow='horizontal'
                      className={
                        this.state.csny && this.state.csny.trim() && this.state.csny !== '请选择'
                          ? 'black-list-extra' : ''
                      }
                      extra={this.state.csny}
                      onClick={() => { this.state.sfsfz === false ? this.csnyxz.show() : '' }}
                    >出生年月</Item>
                    <SyDate
                      onRef={(ref) => { this.csnyxz = ref }}
                      checkDate={(date) => { this.changeDate('csny', date) }}
                      beginDate={this.state.csnydate}
                    />
                    {/* <SYPickerTag
                      dataSource={jcrdjStore.jcrdjxb}
                      disabledTags={this.state.sfsfz ? ['all'] : []}
                      {...getFieldProps('jcrxb', {
                        rules: [{ required: true, message: '请选择性别' }]
                      })}
                    >性别</SYPickerTag> */}
                    { this.state.sfsfz === false
                      ? <SYPickerTag
                        dataSource={jcrdjStore.jcrdjxb}
                        {...getFieldProps('jcrxb', {
                          rules: [{ required: true, message: '请选择性别' }]
                        })}
                      >性别</SYPickerTag>
                      : null
                    }
                  </List>
                  <InputItem
                    type='number'
                    style={{ textAlign: 'right' }}
                    placeholder='请输入'
                    clear
                    {...getFieldProps('jcrsjhm', {
                      rules: [{
                        required: true, message: '请输入手机号码'
                      }, {
                        validator: this.validateJcrdjxx
                      }]
                    })}
                  >手机号码</InputItem>
                  <SYPickerTag
                    dataSource={jcrdjStore.jcrdjhyzk}
                    {...getFieldProps('jcrhyzk', {
                      rules: [{ required: true, message: '请选择婚姻状况' }]
                    })}
                  >婚姻状况</SYPickerTag>
                  <InputItem
                    type='text'
                    style={{ textAlign: 'right' }}
                    placeholder='请输入'
                    clear
                    maxLength='50'
                    {...getFieldProps('jcrjtzz', {
                      rules: [{
                        required: true, message: '请输入家庭住址'
                      }, {
                        min: 10, message: '家庭住址应大于10个字符'
                      }]
                    })}
                  >家庭住址</InputItem>
                  <InputItem
                    type='text'
                    style={{ textAlign: 'right' }}
                    placeholder='请输入'
                    clear
                    maxLength='6'
                    {...getFieldProps('yzbm', {
                      initialValue: this.state.yzbm,
                      rules: [{ validator: this.validateJcrdjxx }]
                    })}
                  >邮政编码</InputItem>
                  <MoneyInput
                    labelNumber='6'
                    clear
                    type='money'
                    placeholder='0.00'
                    onBlur={(v) => { this.getyjce(v) }}
                    {...getFieldProps('grjcjs', {
                      rules: [{ required: true, message: '请输入个人缴存基数' }]
                    })}
                  >缴存基数</MoneyInput>
                  {/* <Item>
                    <Brief
                      {...getFieldProps('gryjce', { rules: [{ required: true, message: '请输入个人月缴存额' }] })}
                    >个人月缴存额：<span>{this.state.gryjce}</span></Brief>
                  </Item>
                  <Item>
                    <Brief
                      {...getFieldProps('dwyjce', { rules: [{ required: true, message: '请输入单位月缴存额' }] })}
                    >单位月缴存额：<span>{this.state.dwyjce}</span></Brief>
                  </Item> */}
                  <InputItem
                    labelNumber='6'
                    clear
                    type='text'
                    editable={false}
                    placeholder='0.00'
                    {...getFieldProps('gryjce', {
                      rules: [{ required: true, message: '请输入个人月缴存额' }]
                    })}
                  >个人月缴存额</InputItem>
                  <InputItem
                    labelNumber='6'
                    clear
                    type='text'
                    editable={false}
                    placeholder='0.00'
                    {...getFieldProps('dwyjce', {
                      rules: [{ required: true, message: '请输入单位月缴存额' }]
                    })}
                  >单位月缴存额</InputItem>
                  {/* <Item
                    arrow='horizontal'
                    extra='详细'
                    onClick={() => { this.jcrZhxxlist('1', '', '') }}
                  >个人账户信息</Item>*/}
                  <InputItem
                    type='text'
                    placeholder='请输入'
                    clear
                    style={{ textAlign: 'right' }}
                    labelNumber='6'
                    maxLength='30'
                    {...getFieldProps('jbzhyhzh', {
                      rules: [{
                        required: true, message: '请输入存款账户号码'
                      }, {
                        validator: this.validateJcrdjxx
                      }]
                    })}
                  >银行账号</InputItem>
                  <Item arrow='horizontal'
                    wrap
                    className={
                      this.state.yhaddmc && this.state.yhaddmc.trim() && this.state.yhaddmc !== '请选择'
                        ? 'black-list-extra bigExtra' : 'bigExtra'
                    }
                    onClick={() => this.setState({ yhsearchModel: true })}
                    extra={this.state.yhaddmc}
                    {...getFieldProps('jbzhkhyh', {
                      rules: [{ required: true, message: '请选择开户银行' }]
                    })}
                  >开户银行</Item>
                  <Accordion accordion style={{ textAlign: 'left' }}
                    onChange={() => setTimeout(() => {
                      this.button.forceUpdate()
                    }, 200)}
                  >
                    <Accordion.Panel header={<Head><span style={{ marginLeft: '14px' }}>更多</span></Head>}>
                      <MoneyInput
                        clear
                        type='money'
                        placeholder='0.00'
                        {...getFieldProps('jtysr', { initialValue: this.state.jtysr })}
                      >家庭月收入</MoneyInput>
                      <Item arrow='horizontal'
                        wrap
                        className={
                          this.state.zwmc && this.state.zwmc.trim() && this.state.zwmc !== '请选择'
                            ? 'black-list-extra bigExtra' : 'bigExtra'
                        }
                        onClick={() => this.setState({ zwsearchModel: true })}
                        extra={this.state.zwmc}
                        {...getFieldProps('jcrzw', { initialValue: this.state.jcrzw })}
                      >职务</Item>
                      <SYPickerTag
                        dataSource={jcrdjStore.jcrdjzgzy}
                        {...getFieldProps('jcrzy', { initialValue: this.state.jcrzy })}
                      >职业</SYPickerTag>
                      <Item arrow='horizontal'
                        wrap
                        className={
                          this.state.zcmc && this.state.zcmc.trim() && this.state.zcmc !== '请选择'
                            ? 'black-list-extra bigExtra' : 'bigExtra'
                        }
                        onClick={() => this.setState({ zcsearchModel: true })}
                        extra={this.state.zcmc}
                        {...getFieldProps('jcrzc', { initialValue: this.state.jcrzc })}
                      >职称</Item>
                      <SYPickerTag
                        dataSource={jcrdjStore.jcrdjzgxl}
                        {...getFieldProps('jcrxl', { initialValue: this.state.jcrxl })}
                      >学历</SYPickerTag>
                      <SYInput
                        kind='Phone'
                        maxLength={20}
                        style={{ textAlign: 'right' }}
                        placeholder='请输入'
                        clear
                        labelNumber='6'
                        {...getFieldProps('jcrgddh', {
                          initialValue: this.state.jcrgddh,
                          rules: [{ validator: this.validateJcrdjxx }]
                        })}
                      >固定电话号码</SYInput>
                      <InputItem
                        type='text'
                        style={{ textAlign: 'right' }}
                        placeholder='请输入'
                        clear
                        maxLength='50'
                        {...getFieldProps('jcrdzyx', {
                          initialValue: this.state.jcrdzyx,
                          rules: [{ validator: this.validateJcrdjxx }]
                        })}
                      >电子邮箱</InputItem>
                    </Accordion.Panel>
                  </Accordion>
                </List>
              </SYCard.Body>
            </SYCard>
            {bbgrxx && bbgrxx.blqd !== 'app_12329' ? <WingBlank>
              <WhiteSpace size='lg' />
              <FixedButton
                onRef={ref => {
                  this.button = ref
                }}
                modalButton
                singleButton
                nextContent='保存'
                nextClick={() => { this.saveJcr() }}
              />
            </WingBlank>
              : <>
                <WhiteSpace size='lg' />
                <FixedButton
                  onRef={ref => {
                    this.button = ref
                  }}
                  modalButton
                  nextContent='保存'
                  nextClick={() => { this.saveJcr() }}
                  preContent='返回'
                  preClick={this.onClose('jcrlrModel')}
                />
              </>
            }
          </div>
        </SyModal>
        <SyModal
          visible={this.state.yhsearchModel}
          animationType='slide-up'
          changeStateTag={(rowData) => this.setState({ yhsearchModel: false })}
          ref={ref => (this.yhsearchModel = ref)}
        >
          <SYLxSearch
            url={API_PATH.GET_HFB_JBZHKHYH_DATA}
            searchKey='mc'
            size={15}
            onCancel={(rowData) => this.setState({ yhsearchModel: false })}
            onSubmit={(rowData) => this.handleYhsearch(rowData)}
            params={{ ywfl: '01', ywlb: '0302' }}
            span={(rowData, sectionID, rowID) => {
              return (<span style={{ fontSize: '0.75rem' }}>{rowData.bm} | {rowData.mc}</span>)
            }}
          />
        </SyModal>
        <SyModal
          visible={this.state.zwsearchModel}
          animationType='slide-up'
          changeStateTag={(rowData) => this.setState({ zwsearchModel: false })}
          ref={ref => (this.zwsearchModel = ref)}
        >
          <SYLxSearch
            url={API_PATH.GET_HFB_TYBM_DATA}
            qtSearchKey={['mc']}
            size={15}
            onCancel={(rowData) => this.setState({ zwsearchModel: false })}
            onSubmit={(rowData) => this.zwSearch(rowData)}
            params={{ zxjgbm: gjjgrxx.zxjgbm, tablename: 'bm_zgzw' }}
            span={(rowData, sectionID, rowID) => {
              return (<span style={{ fontSize: '0.75rem' }}>{rowData.bm} | {rowData.mc}</span>)
            }}
          />
        </SyModal>
        <SyModal
          visible={this.state.zcsearchModel}
          animationType='slide-up'
          changeStateTag={(rowData) => this.setState({ zcsearchModel: false })}
          ref={ref => (this.zcsearchModel = ref)}
        >
          <SYLxSearch
            url={API_PATH.GET_HFB_TYBM_DATA}
            qtSearchKey={['mc']}
            size={15}
            onCancel={(rowData) => this.setState({ zcsearchModel: false })}
            onSubmit={(rowData) => this.zcSearch(rowData)}
            params={{ zxjgbm: gjjgrxx.zxjgbm, tablename: 'bm_zgzc' }}
            span={(rowData, sectionID, rowID) => {
              return (<span style={{ fontSize: '0.75rem' }}>{rowData.bm} | {rowData.mc}</span>)
            }}
          />
        </SyModal>
        <SyModal
          visible={this.state.jcrqcModel}
          animationType='slide-up'
          changeStateTag={this.onClose('jcrqcModel')}
          childModals={[this.jcrlrModel]}
          alert={this.deleteAlert}
        >
          <NavBar
            mode='light'
            icon={<LeftClickIcon />}
            onLeftClick={this.onClose('jcrqcModel')}
          >缴存人登记清册</NavBar>
          <div
            style={{
              position: 'relative',
              top: '3.125rem'
            }}
            className='WPK-noBodyLine'
          >
            <SYListView
              NUM_ROWS={5}
              dataSource={jcrdjStore.jcrdjjcrlist.data}
              renderRow={renderRow}
              onRef={ref => (this.SYListView = ref)}
              height={this.state.ListViewHeight}
            />
            {/* <List style={{ paddingTop: '2.875rem' }}>
              {
                jcrdjStore.jcrdjjcrlist.success ? jcrdjStore.jcrdjjcrlist.data.map((o, i) => {
                  return (
                    <div key={i} style={{ borderBottom: '1px solid #eee', height: '88px' }}>
                      <div style={{ float: 'left', width: '90%' }}>
                        <Item onClick={() => this.jcrmxinfo(o.grzh)} className='noBeforeAndAfterLine'>
                          <Brief style={{ 'color': '#000000', padding: 0 }}>{o.xingming}</Brief>
                          <Brief style={{ padding: 0, marginLeft: '5px', marginBottom: '0.875rem' }}>
                            <Flex direction='row' style={{ padding: 0, marginLeft: '5px' }}>
                              <Flex.Item style={{ flex: 'auto', fontSize: '13px' }}>证件号码：{o.zjhm}</Flex.Item>
                            </Flex>
                            <Flex direction='row' style={{ padding: 0, marginLeft: '5px' }}>
                              <Flex.Item style={{ flex: 'auto', fontSize: '13px' }}>个人缴存基数：{fmoney(o.grjcjs, 2)}</Flex.Item>
                            </Flex>
                          </Brief>
                        </Item>
                      </div>
                      <div style={{ float: 'right', width: '10%' }}>
                        <img src={minus} onClick={() => { this.delJcr(o.grzh) }}
                          style={{ width: '25px', height: '25px', marginTop: '25px' }}
                        ></img>
                      </div>
                    </div>
                  )
                }) : null
              }
            </List>  */}
          </div>
        </SyModal>
        <SyModal
          visible={this.state.jcrmxModel}
          animationType='slide-up'
          changeStateTag={this.onClose2('jcrmxModel')}
        >
          <div style={{ height: '100vh' }}>
            <NavBar
              mode='light'
              icon={<LeftClickIcon />}
              onLeftClick={this.onClose2('jcrmxModel')}
            >缴存人登记明细</NavBar>
            <Jcrdjmx
              {...jcrdjStore.jcrdjjcrinfo}
            // jcrZhxxlist={this.jcrZhxxlist}
            />
            {/* <WingBlank>
              <WhiteSpace size='lg' />
              <Button type='primary' style={{ width: '100%' }} inline onClick={() => { this.delJcr(jcrdjStore.jcrdjjcrinfo.grzh) }}>删除</Button>
            </WingBlank> */}
          </div>
        </SyModal>
        <NavBar
          mode='light'
          icon={<LeftClickIcon />}
          onLeftClick={this.backClick}
          style={bbgrxx && bbgrxx.blqd !== 'app_12329' ? { fontWeight: 'bold', display: this.state.showbt ? 'flex' : 'none', borderBottom: '1px solid #eee' } : { display: 'none' }}
        >缴存人登记</NavBar>
        <div style={bbgrxx && bbgrxx.blqd !== 'app_12329' && bbgrxx.blqd !== 'wx' ? { borderRadius: '6px', paddingTop: '2.875rem' } : { display: 'none' }}>
          <UserInfo />
        </div>
        <div style={bbgrxx && bbgrxx.blqd !== 'app_12329' && bbgrxx.blqd !== 'wx' ? {} : { display: 'none' }} className='am-whitespace am-whitespace-md'></div>
        <div>
          <Events
            sq
            Event='缴存人登记'
            EventKey='缴存单位'
            EventValue={dwmc}
            oprwhx={this.opQyhx}
          />
          <SYCard full showBeforeAndAfterLine>
            <SYCard.Header>内容</SYCard.Header>
            <SYCard.Body>
              <List className='WPK-noBodyLine sylist'>
                <Item arrow='horizontal' wrap extra={<span onClick={this.addJcr} >增加</span>}>
                  缴存人登记人数：<span style={{ color: '#4A90E2' }} onClick={() => { this.openJcrlist() }}>{jcrdjStore.jcrdjjcrlist.totalcount ? jcrdjStore.jcrdjjcrlist.totalcount : 0}人</span>
                </Item>
              </List>
            </SYCard.Body>
          </SYCard>
          <SyUpload
            atBeginOpen
            params={{
              dwzh: dwzh,
              ywlsh: bpmid,
              bpmid: Number(bpmid),
              grlbbm: '01',
              jcblbm: this.state.jcblbm,
              blqd: bbgrxx.blqd,
              userid: gjjgrxx.userid,
              zxjgbm: bbgrxx.zxjgbm,
              zxbm: gjjgrxx.zxbm,
              jgbm: gjjgrxx.jgbm,
              username: gjjgrxx.username,
              zjbzxbm: bbgrxx.zjbzxbm,
              jgmc: bbgrxx.jgmc
            }}
            needErrDownload
            apiName='GET_JCDW_JCRDJ_JCRXXDR'
            errQueryApiName='GET_JCDW_JCRDJ_DRCWXXCX'
            errDownloadApiName='GET_JCDW_JCRDJ_DRCWXXDC'
            templateName='jcr/缴存人登记.xls'
            afterUpload={async() => {
              await jcrdjStore.getJcrdjjcrlist({
                ...jcrdjStore.publicParam,
                bpmid: Number(bpmid),
                page: 1,
                size: 100,
              })
            }}
          />
          <SYCard full showBeforeAndAfterLine>
            <SYCard.Header>材料</SYCard.Header>
            <SYCard.Body style={{ paddingLeft: '14px', paddingBottom: '14px', paddingTop: '0' }}>
              <ImageUploadGroup {...{ easywfl: '01', easywlb: '0302',proccesskey: 'hfb_jcrdjsp' }} onRef={(ref) => { this.childda = ref }} />
            </SYCard.Body>
          </SYCard>
          <SYCard full showBeforeAndAfterLine showWhiteSpace={false}>
            <SYCard.Header>规则</SYCard.Header>
            <SYCard.Body>
              {/* <Rules dataSource={this.state.gzData} /> */}
              {/* <Rules*/}
              {/*  // dataSource={this.state.gzData}*/}
              {/*  {...{*/}
              {/*    ywfl: '01',*/}
              {/*    ywlb: '0302',*/}
              {/*    dwzh: this.state.dwzh,*/}
              {/*    objectservice: 'jcdw',*/}
              {/*    taskDefinitionName: '发起人',*/}
              {/*    lcfx: 1,*/}
              {/*    sfzsf: 1,*/}
              {/*    zjbzxbm: gjjgrxx.zjbzxbm,*/}
              {/*  }}*/}
              {/*  onRef={(ref) => { this.rule = ref }}*/}
              {/* />*/}
              <Rules onRef={(ref) => { this.rule = ref }} />
            </SYCard.Body>
          </SYCard>
        </div>
        <div>
          <FixedButton
            singleButton
            nextContent='提交'
            nextClick={this.goTjsp}
          />
        </div>
        {/* 人物画像 */}
        <SyModal
          visible={this.state.qyhx}
          animationType='slide-up'
          changeStateTag={this.closeQyhx}
        >
          {bbgrxx && bbgrxx.blqd !== 'app_12329'
            ? <NavBar
              mode='light'
              icon={<LeftClickIcon />}
              onLeftClick={this.closeQyhx}
            >企业画像</NavBar>
            : <></>}
          <Qyhx urldata={this.state.urldata} />
          {bbgrxx && bbgrxx.blqd === 'app_12329' && <Flex className='FixedButton' style={{ padding: '0 3% 0.875rem 3%' }}>
            <Flex.Item>
              <Button
                type='warning'
                onClick={this.closeQyhx}
                inline
                style={{
                  fontSize: '0.875rem',
                  width: '-webkit-fill-available'
                }}
              >
                返回
              </Button>
            </Flex.Item>
          </Flex>}
        </SyModal>
      </div >
    )
  }
}
export default jcrdj

```

