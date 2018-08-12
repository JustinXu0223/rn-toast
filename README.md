
```html
import React from 'react';
import styled from 'styled-components';
import RootToast from 'js-rn-toast';

// static source 
import succeedIcon from '../img/succeed.png';
import warningIcon from '../img/warning.png';
import errorIcon from '../img/error.png';

const ContainerView = styled.View`
  width: 181px;
  height: 105px;
  justify-content: center;
  align-items: center;
`;

const ImageIcon = styled.Image`
  width: 39px;
  height: 39px;
`;

const MessageText = styled.Text`
  color: #fff;
  margin-top: 12px;
  text-align: center;
  line-height: 15px;
`;

const SuccessText = MessageText.extend`
  color: #000000;
`;

const Toast = {
  toast: null,
  close(callback) {
    if (!this.toast) return;
    RootToast.hide(this.toast);
    this.toast = null;
    typeof callback === 'function' ? callback && callback() : null;
  },
  clearTime() {
    if (!this.timer) return;
    clearTimeout(this.timer);
    this.timer = null;
  },
  /** 基础Toast配置
   * @param content 内容
   * @param options 配置
   * @param callback 回调函数
  * */
  basicToast(content, { position = 0, duration = 1500, ...restProps } = {}, callback) {
    // only show one
    if (this.toast) return;
    this.toast = RootToast.show(content, {
      position,
      duration,
      onHidden: () => {
        this.close(callback);
        this.clearTime();
      },
      ...restProps,
    });
    // not auto close
    if (duration === 0) return;
    this.timer = setTimeout(() => {
      this.close(callback);
    }, duration);
  },
  showText(msg, options, callback) {
    if (!(typeof msg === 'number' || typeof msg === 'string')) {
      throw new TypeError('type only support number or string');
    }
    msg = msg.toString();
    if (!msg) {
      throw new Error('params cannot include any spaces');
    }
    this.basicToast(msg, options, callback);
  },
  showSuccess(node, options, callback) {
    if (!node && node !== 0) {
      throw new Error('params cannot include any spaces');
    }
    if (!React.isValidElement(node)) {
      node = (
        <ContainerView>
          <ImageIcon source={succeedIcon} />
          <SuccessText>{node}</SuccessText>
        </ContainerView>
      );
    }
    this.basicToast(node, {
      position: RootToast.positions.CENTER,
      backgroundColor: '#ffffff',
      opacity: 1,
      ...options,
    }, callback);
  },
  showWarning(node, options, callback) {
    if (!node && node !== 0) {
      throw new Error('params cannot include any spaces');
    }
    if (!React.isValidElement(node)) {
      node = (
        <ContainerView>
          <ImageIcon source={warningIcon} />
          <MessageText>{node}</MessageText>
        </ContainerView>
      );
    }
    this.basicToast(node, {
      position: RootToast.positions.CENTER,
      ...options,
    }, callback);
  },
  showError(node, options, callback) {
    if (!node && node !== 0) {
      throw new Error('params cannot include any spaces');
    }
    if (!React.isValidElement(node)) {
      node = (
        <ContainerView>
          <ImageIcon source={errorIcon} />
          <MessageText>{node}</MessageText>
        </ContainerView>
      );
    }
    this.basicToast(node, {
      position: RootToast.positions.CENTER,
      backgroundColor: '#000000',
      ...options,
    }, callback);
  },
}

export default Toast;

```
