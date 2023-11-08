# detectEthereumProvider

DApp connects to OneKey Complete Demo

```typescript
interface EthereumProvider {
  isOneKey?: boolean;
  once(eventName: string | symbol, listener: (...args: any[]) => void): this;
  on(eventName: string | symbol, listener: (...args: any[]) => void): this;
  off(eventName: string | symbol, listener: (...args: any[]) => void): this;
  addListener(eventName: string | symbol, listener: (...args: any[]) => void): this;
  removeListener(eventName: string | symbol, listener: (...args: any[]) => void): this;
  removeAllListeners(event?: string | symbol): this;
}

interface Window {
  ethereum?: EthereumProvider;
  $onekey?: {
    ethereum?: EthereumProvider;
  }
}

function detectEthereumProvider<T = EthereumProvider>({
  mustBeOneKey = false,
  silent = false,
  timeout = 3000,
} = {}): Promise<T | null> {

  _validateInputs();

  let handled = false;

  return new Promise((resolve) => {
    if ((window as Window).ethereum || (window as Window).$onekey?.ethereum) {
      handleEthereum();
    } else {
      window.addEventListener(
        'ethereum#initialized',
        handleEthereum,
        { once: true },
      );

      setTimeout(() => {
        handleEthereum();
      }, timeout);
    }

    function handleEthereum() {
      if (handled) {
        return;
      }
      handled = true;

      window.removeEventListener('ethereum#initialized', handleEthereum);
      const ethereum = ((window as Window).$onekey && (window as Window).$onekey.ethereum) || (window as Window).ethereum;

      if (ethereum && (!mustBeOneKey || ethereum.isOneKey)) {
        resolve(ethereum as T);
      } else {
        const message = mustBeOneKey && ethereum
          ? 'Non-OneKey window.ethereum detected.'
          : 'Unable to detect window.ethereum.';

        !silent && console.error('Detect Ethereum Provider:', message);
        resolve(null);
      }
    }
  });

  function _validateInputs() {
    if (typeof mustBeOneKey !== 'boolean') {
      throw new Error('Detect Ethereum Provider: Expected option "mustBeOneKey" to be a boolean.');
    }
    if (typeof silent !== 'boolean') {
      throw new Error('Detect Ethereum Provider: Expected option "silent" to be a boolean.');
    }
    if (typeof timeout !== 'number') {
      throw new Error('Detect Ethereum Provider: Expected option "timeout" to be a number.');
    }
  }
}

export default detectEthereumProvider;
```
