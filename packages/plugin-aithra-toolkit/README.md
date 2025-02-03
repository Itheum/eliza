# Aithra Music Playlist cNFT Minter

Transform your audio tracks and images into NFT music playlists with just a few lines of code! This plugin simplifies the process of creating and minting music cNFTs by handling all the complex operations behind the scenes.

> **Note**: You'll need SOL in your wallet to cover the costs of decentralized storage and cNFT minting on the Solana blockchain.

## Features

-   Simple API for creating music NFTs
-   Support for single and multiple track uploads
-   Custom playlist metadata handling
-   Automatic animation file processing
-   Built-in storage management

## Quick Start

```typescript
// Initialize the service
const itheumService = runtime.getService(ServiceType.AITHRA_TOOLKIT);
await itheumService.initialize(runtime, "./temp");

// Load your audio and image files
const audioBuffer = await fs.promises.readFile("./path/to/audio.mp3");
const imageBuffer = await fs.promises.readFile("./path/to/cover.png");
const animationBuffer = await fs.promises.readFile("./path/to/animation.jpg");

// Generate cover image
const base64Image = await generateImage(
    {
        prompt: coverPromt,
        width: 512,
        height: 512,
        count: 1,
    },
    runtime
);

const imageBuffer = Buffer.from(base64Image, "base64");

const base64Animation = await generateImage(
    {
        prompt: animationPrompt,
        width: 512,
        height: 512,
        count: 1,
    },
    runtime
);

const animationBuffer = Buffer.from(base64Animation, "base64");

// Store a single track
await itheumService.storeTrackToFolder({
    track: {
        data: audioBuffer,
        metadata: {
            artist: "Artist Name",
            album: "Album Name",
            title: "Track Title",
            category: "Music Category",
        },
        image: imageBuffer,
    },
});

const animationPath = await itheumService.storeAnimationToFolder({
    animation: animationBuffer,
    extension: "jpg",
});

// Create and mint your NFT playlist
const response = await itheumService.buildUploadMintMusicNFTs({
    playlist: {
        name: "My Awesome Playlist",
        creator: "Creator Name",
    },
    nft: {
        tokenName: "AWESOME_NFT",
        sellerFeeBasisPoints: 50,
        quantity: 1,
        name: "Awesome Music Collection",
        description: "A unique collection of amazing tracks",
    },
    animation: {
        animationFile: animationPath,
    },
});
```

## Detailed Usage

### 1. Initializing the Service

```typescript
const itheumService = runtime.getService(ServiceType.AITHRA_TOOLKIT);
await itheumService.initialize(runtime, "./temp");
```

### 2. Storing Tracks

#### Single Track

```typescript
await itheumService.storeTrackToFolder({
    track: {
        data: audioBuffer,
        metadata: {
            artist: "Artist Name",
            album: "Album Name",
            title: "Track Title",
            category: "Category",
        },
        image: imageBuffer,
    },
});
```

#### Multiple Tracks

```typescript
await itheumService.storeTracksToFolder({
    tracks: [
        {
            data: audioBuffer,
            metadata: {
                artist: "Artist Name",
                album: "Album Name",
                title: "Track Title",
                category: "Category",
            },
            image: imageBuffer,
        },
    ],
});
```

### 3. Storing Animation

```typescript
const animationPath = await itheumService.storeAnimationToFolder({
    animation: animationBuffer,
    extension: "jpg",
});
```

### 4. Creating and Minting NFT Playlist

```typescript
const response = await itheumService.buildUploadMintMusicNFTs({
    playlist: {
        name: "Playlist Name",
        creator: "Creator Name",
    },
    nft: {
        tokenName: "UNIQUE_TOKEN_NAME",
        sellerFeeBasisPoints: 50, // 0.5% seller fee
        quantity: 1,
        name: "NFT Display Name",
        description: "NFT Description",
    },
    animation: {
        animationFile: animationPath,
    },
});
```
