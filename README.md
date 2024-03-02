# Megaphone Client

Typescript and Javascript library to subscribe to [Megaphone](https://github.com/dghilardi/megaphone) channels.

## Usage

Example usage:

```ts
import { MegaphonePoller } from 'megaphone-client';
import { firstValueFrom } from 'rxjs';

const poller = new MegaphonePoller('http://localhost:3000', 100);
const o = await poller.newUnboundedStream<{ message: string, sender: string }>(async channel => {
    let res = await fetch('http://localhost:3040/room/test', {
        method: 'POST',
        headers: {
            ...(channel ? { 'use-channel': channel } : {}),
        },
    }).then(res => res.json());

    return {
        channelAddress: { consumer: res.channelUuid, producer: '' },
        streamIds: ['new-message'],
    }
});

const chunk = await firstValueFrom(o);
console.log(chunk.body.message);
```

See [Megaphone demo](https://github.com/dghilardi/megaphone-demo) for more complete examples.