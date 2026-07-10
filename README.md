// ... existing code ...
import React, { useState, useEffect } from 'react';
// 🌟 MapPin（ピンのマーク）と ShoppingCart（カートのマーク）のアイコンを追加しました
import { Check, ChevronRight, RefreshCw, Search, Sun, Wind, Sparkles, Droplets, MapPin, ShoppingCart, ThumbsUp } from 'lucide-react';

// 🌟 店舗名を後から簡単に変えられるように変数化しておきます
const STORE_NAME = "草加店";

const RESULTS = {
  cotton: {
    name: '綿',
    icon: <Sun className="w-12 h-12 text-orange-500 mb-2" />,
    catchphrase: '肌への優しさと吸水性はNo.1！',
    description: 'オールシーズン快適に使える王道のシーツです。寝汗をしっかり吸い取り、自然なやわらかさであなたを包み込みます。「オーガニックコットン」や「ホテルスタイル」など選択肢も豊富です。',
    searchQuery: 'マルチすっぽりシーツ 綿',
    // 🌟 いただいた商品と通路のデータを追加
    products: [
      { name: 'マルチすっぽりシーツ2 コットンウォッシュ', code: '7518116', aisle: '123' },
      { name: 'マルチすっぽりシーツ ニット', code: '7524384', aisle: '122' },
      { name: 'マルチすっぽりシーツMJ06', code: '2115100101101', aisle: '124' }
    ]
  },
  linen: {
    name: '麻',
    icon: <Wind className="w-12 h-12 text-teal-600 mb-2" />,
    catchphrase: '通気性と吸水速乾性はトップクラス！',
    description: 'サラッとしたシャリ感があり、汗をかいても肌に張り付きません。ニトリの「フレンチリネン混」などは、使い込むほどに柔らかく肌に馴染む、育てていくシーツです。',
    searchQuery: 'マルチすっぽりシーツ 麻',
    products: [
      { name: 'マルチすっぽりシーツリネン', code: '7524477', aisle: '121' }
    ]
  },
  poly: {
    name: 'ポリエステル',
    icon: <Sparkles className="w-12 h-12 text-blue-500 mb-2" />,
    catchphrase: '圧倒的な扱いやすさとコスパ！',
    description: '「シワになりにくい」「乾きやすい」加工がされているものが多く、忙しい毎日の家事の時短になります。柄やカラー展開も豊富で、お部屋の模様替えにも最適です。',
    searchQuery: 'マルチすっぽりシーツ ポリエステル',
    products: [
      { name: 'マルチすっぽりシーツ38', code: '7526244', aisle: '120' },
      { name: 'マルチすっぽりシーツMJ04', code: '2115100019888', aisle: '121' },
      { name: 'マルチすっぽりシーツMJ07', code: '2115100117881', aisle: '306' }
    ]
  },
  rayon: {
    name: 'レーヨン',
    icon: <Droplets className="w-12 h-12 text-indigo-500 mb-2" />,
    catchphrase: 'しっとりなめらかな肌触りと究極の機能性！',
    description: '「Nクール(接触冷感)」のなめらかさや、「Nウォーム(吸湿発熱)」のじんわりとした暖かさを生み出す魔法の素材。季節のお悩み解決と、極上のとろみ感を両立したい方に最適です。',
    searchQuery: 'マルチすっぽりシーツ レーヨン Nシリーズ',
    products: [
      { name: 'マルチすっぽりシーツNfit', code: '2115100003610', aisle: '121' }
    ]
  }
};

// --- コンポーネント ---
// ... existing code ...
  const handleUnderstood = () => {
    if (typeof window !== 'undefined' && window.gtag) {
      window.gtag('event', 'click_understood');
    }
    setHasUnderstood(true);
  };

  // 🌟 個別商品の商品コード（productCode）を受け取れるように改修
  const handleNitoriNetClick = (productCode = null) => {
    if (typeof window !== 'undefined' && window.gtag) {
      // 全体の送客数カウント用イベント
      window.gtag('event', 'click_nitorinet'); 
      // どの商品がクリックされたかを記録するイベント
      if (productCode) {
        window.gtag('event', `click_nitorinet_${productCode}`);
      }
    }
    
    // 引数に商品コードがあれば、その商品の直リンクへ飛ぶ
    if (productCode) {
      window.open(`https://www.nitori-net.jp/ec/product/${productCode}/`, '_blank');
    } else {
      // 引数がなければ、今まで通りの検索一覧ページへ飛ぶ
      const query = RESULTS[result].searchQuery;
      window.open(`https://www.nitori-net.jp/ec/search/?q=${encodeURIComponent(query)}`, '_blank');
    }
  };

  const resetQuiz = () => {
// ... existing code ...
        <div className="bg-white p-6 rounded-2xl shadow-lg text-center border-2 border-teal-500 mb-6">
          {RESULTS[result].icon}
          <h2 className="text-2xl font-bold text-gray-800 mb-2">あなたにぴったりの素材は「{RESULTS[result].name}」</h2>
          <p className="text-teal-600 font-bold mb-4">{RESULTS[result].catchphrase}</p>
          <p className="text-gray-600 text-sm leading-relaxed mb-4 text-left">
            {RESULTS[result].description}
          </p>
        </div>

        {/* 🌟 新規追加：草加店のおすすめ商品と売場案内セクション */}
        <div className="mb-6">
          <h3 className="text-lg font-bold text-gray-800 mb-4 flex items-center justify-center">
            <MapPin className="w-5 h-5 text-red-500 mr-1" />
            {STORE_NAME}のおすすめ商品と売場
          </h3>
          <div className="space-y-3">
            {RESULTS[result].products.map((product, index) => (
              <div key={index} className="bg-white p-4 rounded-xl shadow-sm border border-gray-200 text-left relative overflow-hidden">
                {/* アクセントカラーのライン */}
                <div className="absolute left-0 top-0 bottom-0 w-1 bg-teal-500"></div>
                
                <div className="flex justify-between items-start mb-1 pl-2">
                  <h4 className="font-bold text-gray-800 text-sm pr-2 leading-tight">{product.name}</h4>
                  <span className="bg-red-100 text-red-700 text-xs font-bold px-2 py-1 rounded whitespace-nowrap shadow-sm border border-red-200">
                    📍 {product.aisle}番通路
                  </span>
                </div>
                <p className="text-xs text-gray-400 mb-3 pl-2">商品コード: {product.code}</p>
                <div className="pl-2">
                  <button
                    onClick={() => handleNitoriNetClick(product.code)}
                    className="w-full bg-blue-50 hover:bg-blue-100 text-blue-700 font-bold py-2 px-4 rounded-lg flex items-center justify-center transition-colors text-sm border border-blue-200"
                  >
                    <ShoppingCart className="w-4 h-4 mr-2" />
                    この商品をネットで見る
                  </button>
                </div>
              </div>
            ))}
          </div>
        </div>

        <div className="space-y-3">
          <button
// ... existing code ...
          <button
            onClick={() => handleNitoriNetClick()}
            className="w-full bg-white hover:bg-gray-50 text-teal-600 font-bold py-4 px-6 rounded-xl shadow border-2 border-teal-500 flex items-center justify-center transition-colors"
          >
            <Search className="w-5 h-5 mr-2" />
            ニトリネットで{RESULTS[result].name}をすべて見る
          </button>
        </div>
      </div>
    </div>
// ... existing code ...
