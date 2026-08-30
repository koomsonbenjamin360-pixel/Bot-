import React, { useEffect, useState } from 'react';
import {
  SafeAreaView,
  View,
  Text,
  StyleSheet,
  TouchableOpacity,
  TextInput,
  ScrollView,
  StatusBar,
  Dimensions,
} from 'react-native';

import { getMarketData } from './marketData';

const { width } = Dimensions.get('window');

const ASSETS = [
  {
    symbol: 'EUR/USD',
    name: 'Euro / US Dollar',
    category: 'Forex',
    change: '+0.23%',
    positive: true,
  },
  {
    symbol: 'GBP/USD',
    name: 'British Pound / US Dollar',
    category: 'Forex',
    change: '+0.17%',
    positive: true,
  },
  {
    symbol: 'USD/JPY',
    name: 'US Dollar / Japanese Yen',
    category: 'Forex',
    change: '-0.12%',
    positive: false,
  },
  {
    symbol: 'AUD/USD',
    name: 'Australian Dollar / US Dollar',
    category: 'Forex',
    change: '-0.10%',
    positive: false,
  },
  {
    symbol: 'XAU/USD',
    name: 'Gold / US Dollar',
    category: 'Metals',
    change: '+0.32%',
    positive: true,
  },
  {
    symbol: 'BTC/USD',
    name: 'Bitcoin / US Dollar',
    category: 'Crypto',
    change: '+1.42%',
    positive: true,
  },
];

const TIMEFRAMES = [
  '1m',
  '5m',
  '15m',
  '30m',
  '1H',
  '4H',
  '1D',
];

const CATEGORIES = [
  'All',
  'Forex',
  'Metals',
  'Crypto',
];

export default function App() {
  const [screen, setScreen] = useState('Dashboard');
  const [selectedAsset, setSelectedAsset] =
    useState(ASSETS[0]);

  const [timeframe, setTimeframe] =
    useState('15m');

  const [favorites, setFavorites] = useState([
    'EUR/USD',
    'XAU/USD',
  ]);

  const [search, setSearch] = useState('');
  const [category, setCategory] = useState('All');

  const goTo = (page) => {
    setScreen(page);
  };

  const openChart = (asset) => {
    setSelectedAsset(asset);
    setScreen('Charts');
  };

  const toggleFavorite = (symbol) => {
    setFavorites((old) => {
      if (old.includes(symbol)) {
        return old.filter((x) => x !== symbol);
      }

      return [...old, symbol];
    });
  };

  const filteredAssets = ASSETS.filter((asset) => {
    const matchesSearch =
      asset.symbol
        .toLowerCase()
        .includes(search.toLowerCase()) ||
      asset.name
        .toLowerCase()
        .includes(search.toLowerCase());

    const matchesCategory =
      category === 'All' ||
      asset.category === category;

    return matchesSearch && matchesCategory;
  });

  return (
    <SafeAreaView style={styles.container}>
      <StatusBar
        barStyle="light-content"
        backgroundColor="#05070B"
      />

      {screen === 'Dashboard' && (
        <Dashboard
          goTo={goTo}
          favorites={favorites}
          openChart={openChart}
        />
      )}

      {screen === 'Markets' && (
        <Markets
          assets={filteredAssets}
          search={search}
          setSearch={setSearch}
          category={category}
          setCategory={setCategory}
          favorites={favorites}
          toggleFavorite={toggleFavorite}
          openChart={openChart}
        />
      )}

      {screen === 'Charts' && (
        <ChartScreen
          asset={selectedAsset}
          timeframe={timeframe}
          setTimeframe={setTimeframe}
          favorites={favorites}
          toggleFavorite={toggleFavorite}
          goTo={goTo}
        />
      )}

      {screen === 'AI Analyzer' && (
        <AIAnalyzer goTo={goTo} />
      )}

      {screen === 'Trade' && (
        <TradeScreen
          asset={selectedAsset}
          goTo={goTo}
        />
      )}

      {screen === 'Positions' && (
        <SimpleScreen
          icon="📂"
          title="Positions"
          text="Your open positions will appear here."
          goTo={goTo}
        />
      )}

      {screen === 'History' && (
        <SimpleScreen
          icon="📜"
          title="History"
          text="Your completed trades will appear here."
          goTo={goTo}
        />
      )}

      {screen === 'Alerts' && (
        <SimpleScreen
          icon="🔔"
          title="Alerts"
          text="Your market alerts will appear here."
          goTo={goTo}
        />
      )}

      {screen === 'Learning' && (
        <SimpleScreen
          icon="📚"
          title="Learning"
          text="Learn BOS, CHOCH, EQH, EQL, LQS, FVG, order blocks and market structure."
          goTo={goTo}
        />
      )}

      {screen === 'Settings' && (
        <SimpleScreen
          icon="⚙️"
          title="Settings"
          text="App settings and preferences."
          goTo={goTo}
        />
      )}

      <BottomNavigation
        screen={screen}
        goTo={goTo}
      />
    </SafeAreaView>
  );
}

/* ================= DASHBOARD ================= */

function Dashboard({
  goTo,
  favorites,
  openChart,
}) {
  return (
    <ScrollView
      style={styles.flex}
      contentContainerStyle={styles.content}
    >
      <View style={styles.header}>
        <View>
          <Text style={styles.logo}>
            REC
            <Text style={styles.blue}>K</Text>
            ON
          </Text>

          <Text style={styles.tagline}>
            TRADE • ANALYZE • LEARN
          </Text>
        </View>

        <TouchableOpacity
          style={styles.iconButton}
          onPress={() => goTo('Alerts')}
        >
          <Text>🔔</Text>
        </TouchableOpacity>
      </View>

      <View style={styles.accountCard}>
        <Text style={styles.muted}>
          Demo Account
        </Text>

        <Text style={styles.smallLabel}>
          Balance
        </Text>

        <Text style={styles.balance}>
          $10,625.50
        </Text>

        <View style={styles.rowBetween}>
          <View>
            <Text style={styles.smallLabel}>
              Equity
            </Text>

            <Text style={styles.white}>
              $10,856.35
            </Text>
          </View>

          <View>
            <Text style={styles.smallLabel}>
              Profit
            </Text>

            <Text style={styles.green}>
              +$230.85
            </Text>
          </View>
        </View>
      </View>

      <Text style={styles.sectionTitle}>
        Quick Actions
      </Text>

      <View style={styles.quickRow}>
        <QuickButton
          icon="📊"
          title="Markets"
          onPress={() => goTo('Markets')}
        />

        <QuickButton
          icon="📈"
          title="Charts"
          onPress={() => openChart(ASSETS[0])}
        />

        <QuickButton
          icon="🤖"
          title="AI"
          onPress={() => goTo('AI Analyzer')}
        />
      </View>

      <Text style={styles.sectionTitle}>
        Favorites
      </Text>

      {ASSETS.filter((a) =>
        favorites.includes(a.symbol)
      ).map((asset) => (
        <TouchableOpacity
          key={asset.symbol}
          style={styles.favoriteRow}
          onPress={() => openChart(asset)}
        >
          <View>
            <Text style={styles.white}>
              {asset.symbol}
            </Text>

            <Text style={styles.mutedSmall}>
              {asset.name}
            </Text>
          </View>

          <Text
            style={[
              styles.change,
              asset.positive
                ? styles.green
                : styles.red,
            ]}
          >
            {asset.change}
          </Text>
        </TouchableOpacity>
      ))}

      <View style={styles.aiCard}>
        <Text style={styles.aiTitle}>
          🤖 RECKON AI
        </Text>

        <Text style={styles.aiText}>
          Send a screenshot of a trading chart
          for RECKON AI to analyze market
          structure and liquidity.
        </Text>

        <Text style={styles.aiFeatures}>
          HH • HL • LH • LL
        </Text>

        <Text style={styles.aiFeatures}>
          BOS • CHOCH • EQH • EQL
        </Text>

        <Text style={styles.aiFeatures}>
          LQS • FVG • OB • Engulfing
        </Text>

        <TouchableOpacity
          style={styles.primaryButton}
          onPress={() => goTo('AI Analyzer')}
        >
          <Text style={styles.buttonText}>
            Analyze Chart
          </Text>
        </TouchableOpacity>
      </View>
    </ScrollView>
  );
}

/* ================= MARKETS ================= */

function Markets({
  assets,
  search,
  setSearch,
  category,
  setCategory,
  favorites,
  toggleFavorite,
  openChart,
}) {
  return (
    <View style={styles.screen}>
      <View style={styles.topHeader}>
        <View>
          <Text style={styles.screenTitle}>
            Markets
          </Text>

          <Text style={styles.muted}>
            Watchlist & Quotes
          </Text>
        </View>

        <Text style={styles.live}>
          ● LIVE
        </Text>
      </View>

      <View style={styles.searchBox}>
        <Text>🔍</Text>

        <TextInput
          style={styles.searchInput}
          value={search}
          onChangeText={setSearch}
          placeholder="Search symbol"
          placeholderTextColor="#667085"
        />
      </View>

      <ScrollView
        horizontal
        showsHorizontalScrollIndicator={false}
        style={{ marginBottom: 10 }}
      >
        {CATEGORIES.map((item) => (
          <TouchableOpacity
            key={item}
            style={[
              styles.category,
              category === item &&
                styles.categoryActive,
            ]}
            onPress={() => setCategory(item)}
          >
            <Text
              style={[
                styles.categoryText,
                category === item &&
                  styles.categoryTextActive,
              ]}
            >
              {item}
            </Text>
          </TouchableOpacity>
        ))}
      </ScrollView>

      <ScrollView
        showsVerticalScrollIndicator={false}
      >
        {assets.map((asset) => (
          <TouchableOpacity
            key={asset.symbol}
            style={styles.marketRow}
            onPress={() => openChart(asset)}
          >
            <TouchableOpacity
              style={styles.star}
              onPress={() =>
                toggleFavorite(asset.symbol)
              }
            >
              <Text style={styles.starText}>
                {favorites.includes(asset.symbol)
                  ? '★'
                  : '☆'}
              </Text>
            </TouchableOpacity>

            <View style={styles.assetInfo}>
              <Text style={styles.white}>
                {asset.symbol}
              </Text>

              <Text style={styles.mutedSmall}>
                {asset.name}
              </Text>
            </View>

            <Text
              style={[
                styles.change,
                asset.positive
                  ? styles.green
                  : styles.red,
              ]}
            >
              {asset.change}
            </Text>
          </TouchableOpacity>
        ))}
      </ScrollView>
    </View>
  );
}

/* ================= CHART ================= */

function ChartScreen({
  asset,
  timeframe,
  setTimeframe,
  favorites,
  toggleFavorite,
  goTo,
}) {
  const [candles, setCandles] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState('');

  useEffect(() => {
    loadChart();

    const timer = setInterval(
      loadChart,
      60000
    );

    return () => clearInterval(timer);
  }, [asset.symbol, timeframe]);

  async function loadChart() {
    try {
      setLoading(true);
      setError('');

      const data = await getMarketData(
        asset.symbol,
        timeframe,
        60
      );

      if (!data || data.length === 0) {
        throw new Error(
          'No candle data received.'
        );
      }

      setCandles(data);
    } catch (e) {
      console.log(e);

      setError(
        e?.message ||
          'Unable to load chart data.'
      );
    } finally {
      setLoading(false);
    }
  }

  const last =
    candles.length > 0
      ? candles[candles.length - 1]
      : null;

  const prices = candles.flatMap((c) => [
    Number(c.high),
    Number(c.low),
  ]);

  const max =
    prices.length > 0
      ? Math.max(...prices)
      : 1;

  const min =
    prices.length > 0
      ? Math.min(...prices)
      : 0;

  const range = max - min || 1;

  const chartHeight = 300;
  const chartWidth = width - 65;

  const getY = (price) =>
    ((max - price) / range) *
      (chartHeight - 30) +
    15;

  return (
    <View style={styles.chartScreen}>
      <View style={styles.chartHeader}>
        <TouchableOpacity
          onPress={() => goTo('Markets')}
        >
          <Text style={styles.back}>
            ‹
          </Text>
        </TouchableOpacity>

        <View style={{ flex: 1 }}>
          <Text style={styles.chartTitle}>
            {asset.symbol}
          </Text>

          <Text style={styles.mutedSmall}>
            {timeframe} • Live Market
          </Text>
        </View>

        <TouchableOpacity
          onPress={() =>
            toggleFavorite(asset.symbol)
          }
        >
          <Text style={styles.starText}>
            {favorites.includes(asset.symbol)
              ? '★'
              : '☆'}
          </Text>
        </TouchableOpacity>
      </View>

      <ScrollView
        horizontal
        showsHorizontalScrollIndicator={false}
        style={styles.timeframeRow}
      >
        {TIMEFRAMES.map((tf) => (
          <TouchableOpacity
            key={tf}
            style={[
              styles.tfButton,
              timeframe === tf &&
                styles.tfActive,
            ]}
            onPress={() =>
              setTimeframe(tf)
            }
          >
            <Text
              style={[
                styles.tfText,
                timeframe === tf &&
                  styles.tfTextActive,
              ]}
            >
              {tf}
            </Text>
          </TouchableOpacity>
        ))}
      </ScrollView>

      <View style={styles.chartTools}>
        <Text style={styles.tool}>✋</Text>
        <Text style={styles.tool}>╱</Text>
        <Text style={styles.tool}>━</Text>

        <TouchableOpacity
          onPress={loadChart}
          style={styles.refresh}
        >
          <Text style={styles.tool}>
            ↻
          </Text>
        </TouchableOpacity>
      </View>

      {loading && candles.length === 0 ? (
        <View style={styles.center}>
          <Text style={styles.white}>
            Loading {asset.symbol}...
          </Text>

          <Text style={styles.muted}>
            Getting {timeframe} candles
          </Text>
        </View>
      ) : error &&
        candles.length === 0 ? (
        <View style={styles.center}>
          <Text style={styles.red}>
            Chart Error
          </Text>

          <Text
            style={[
              styles.muted,
              { textAlign: 'center' },
            ]}
          >
            {error}
          </Text>

          <TouchableOpacity
            style={styles.retry}
            onPress={loadChart}
          >
            <Text style={styles.buttonText}>
              Retry
            </Text>
          </TouchableOpacity>
        </View>
      ) : (
        <>
          <View
            style={[
              styles.chartArea,
              { height: chartHeight },
            ]}
          >
            <View style={styles.priceLabels}>
              {[0, 1, 2, 3, 4].map(
                (i) => {
                  const price =
                    max -
                    (range / 4) * i;

                  return (
                    <Text
                      key={i}
                      style={
                        styles.priceLabel
                      }
                    >
                      {formatPrice(
                        price,
                        asset.symbol
                      )}
                    </Text>
                  );
                }
              )}
            </View>

            <View
              style={[
                styles.candleArea,
                {
                  width: chartWidth,
                  height: chartHeight,
                },
              ]}
            >
              {[0, 1, 2, 3, 4].map(
                (i) => (
                  <View
                    key={i}
                    style={[
                      styles.grid,
                      {
                        top:
                          15 +
                          ((chartHeight -
                            30) /
                            4) *
                            i,
                      },
                    ]}
                  />
                )
              )}

              {candles.map(
                (candle, index) => {
                  const x =
                    index *
                    ((chartWidth - 10) /
                      candles.length);

                  const highY = getY(
                    Number(candle.high)
                  );

                  const lowY = getY(
                    Number(candle.low)
                  );

                  const openY = getY(
                    Number(candle.open)
                  );

                  const closeY = getY(
                    Number(candle.close)
                  );

                  const top = Math.min(
                    openY,
                    closeY
                  );

                  const height = Math.max(
                    Math.abs(
                      openY - closeY
                    ),
                    2
                  );

                  const bullish =
                    Number(candle.close) >=
                    Number(candle.open);

                  return (
                    <View
                      key={index}
                      style={[
                        styles.candle,
                        {
                          left: x,
                          height:
                            chartHeight,
                        },
                      ]}
                    >
                      <View
                        style={[
                          styles.wick,
                          {
                            top: highY,
                            height:
                              Math.max(
                                lowY -
                                  highY,
                                1
                              ),
                            backgroundColor:
                              bullish
                                ? '#35D07F'
                                : '#FF5C68',
                          },
                        ]}
                      />

                      <View
                        style={[
                          styles.body,
                          {
                            top,
                            height,
                            backgroundColor:
                              bullish
                                ? '#35D07F'
                                : '#FF5C68',
                          },
                        ]}
                      />
                    </View>
                  );
                }
              )}
            </View>
          </View>

          <View style={styles.analysisCard}>
            <Text style={styles.aiTitle}>
              🤖 RECKON MARKET ANALYSIS
            </Text>

            <View style={styles.analysisGrid}>
              <Analysis
                title="HH"
                value="Scanning"
              />

              <Analysis
                title="HL"
                value="Scanning"
              />

              <Analysis
                title="LH"
                value="Scanning"
              />

              <Analysis
                title="LL"
                value="Scanning"
              />

              <Analysis
                title="BOS"
                value="Scanning"
              />

              <Analysis
                title="CHOCH"
                value="Scanning"
              />

              <Analysis
                title="EQH"
                value="Scanning"
              />

              <Analysis
                title="EQL"
                value="Scanning"
              />

              <Analysis
                title="LQS"
                value="Scanning"
              />
            </View>

            <Text style={styles.mutedSmall}>
              Detection engine will use the
              candle data to identify these
              structures.
            </Text>
          </View>
        </>
      )}

      <View style={styles.tradeBar}>
        <TouchableOpacity
          style={styles.sell}
          onPress={() => goTo('Trade')}
        >
          <Text style={styles.buttonText}>
            SELL
          </Text>
        </TouchableOpacity>

        <TouchableOpacity
          style={styles.buy}
          onPress={() => goTo('Trade')}
        >
          <Text style={styles.buttonText}>
            BUY
          </Text>
        </TouchableOpacity>
      </View>
    </View>
  );
}

/* ================= AI ================= */

function AIAnalyzer({ goTo }) {
  return (
    <View style={styles.center}>
      <Text style={styles.bigIcon}>
        🤖
      </Text>

      <Text style={styles.bigTitle}>
        RECKON AI
      </Text>

      <Text style={styles.aiText}>
        Send a screenshot of your trading
        chart for analysis.
        {'\n\n'}
        The planned AI engine will analyze:
        {'\n'}
        HH • HL • LH • LL
        {'\n'}
        BOS • CHOCH
        {'\n'}
        EQH • EQL • LQS
        {'\n'}
        FVG • Order Blocks
        {'\n'}
        Engulfing • Pullback
      </Text>

      <TouchableOpacity
        style={styles.primaryButton}
      >
        <Text style={styles.buttonText}>
          📷 Upload Chart
        </Text>
      </TouchableOpacity>

      <TouchableOpacity
        style={styles.secondaryButton}
        onPress={() => goTo('Dashboard')}
      >
        <Text style={styles.muted}>
          ← Dashboard
        </Text>
      </TouchableOpacity>
    </View>
  );
}

/* ================= TRADE ================= */

function TradeScreen({ asset, goTo }) {
  return (
    <View style={styles.center}>
      <Text style={styles.bigIcon}>
        💹
      </Text>

      <Text style={styles.bigTitle}>
        Trade
      </Text>

      <Text style={styles.aiText}>
        Selected asset:
        {'\n\n'}
        {asset.symbol}
        {'\n\n'}
        This is currently a demo trading
        screen. Live broker connection will
        be added separately.
      </Text>

      <TouchableOpacity
        style={styles.secondaryButton}
        onPress={() => goTo('Charts')}
      >
        <Text style={styles.muted}>
          ← Back to Chart
        </Text>
      </TouchableOpacity>
    </View>
  );
}

/* ================= SIMPLE ================= */

function SimpleScreen({
  icon,
  title,
  text,
  goTo,
}) {
  return (
    <View style={styles.center}>
      <Text style={styles.bigIcon}>
        {icon}
      </Text>

      <Text style={styles.bigTitle}>
        {title}
      </Text>

      <Text style={styles.aiText}>
        {text}
      </Text>

      <TouchableOpacity
        style={styles.secondaryButton}
        onPress={() => goTo('Dashboard')}
      >
        <Text style={styles.muted}>
          ← Dashboard
        </Text>
      </TouchableOpacity>
    </View>
  );
}

/* ================= ANALYSIS ================= */

function Analysis({ title, value }) {
  return (
    <View style={styles.analysisItem}>
      <Text style={styles.mutedSmall}>
        {title}
      </Text>

      <Text style={styles.whiteSmall}>
        {value}
      </Text>
    </View>
  );
}

/* ================= QUICK BUTTON ================= */

function QuickButton({
  icon,
  title,
  onPress,
}) {
  return (
    <TouchableOpacity
      style={styles.quickButton}
      onPress={onPress}
    >
      <Text style={styles.quickIcon}>
        {icon}
      </Text>

      <Text style={styles.quickText}>
        {title}
      </Text>
    </TouchableOpacity>
  );
}

/* ================= NAVIGATION ================= */

function BottomNavigation({
  screen,
  goTo,
}) {
  const items = [
    ['Dashboard', '⌂', 'Home'],
    ['Markets', '▥', 'Markets'],
    ['Charts', '⌁', 'Chart'],
    ['Trade', '↕', 'Trade'],
    ['Settings', '☰', 'Menu'],
  ];

  return (
    <View style={styles.bottomNav}>
      {items.map(([name, icon, label]) => {
        const active =
          screen === name;

        return (
          <TouchableOpacity
            key={name}
            style={styles.navItem}
            onPress={() => goTo(name)}
          >
            <Text
              style={[
                styles.navIcon,
                active && styles.navActive,
              ]}
            >
              {icon}
            </Text>

            <Text
              style={[
                styles.navText,
                active && styles.navActive,
              ]}
            >
              {label}
            </Text>
          </TouchableOpacity>
        );
      })}
    </View>
  );
}

/* ================= PRICE ================= */

function formatPrice(price, symbol) {
  const number = Number(price);

  if (!Number.isFinite(number)) {
    return '--';
  }

  if (symbol === 'BTC/USD') {
    return number.toFixed(2);
  }

  if (symbol === 'XAU/USD') {
    return number.toFixed(2);
  }

  if (symbol === 'USD/JPY') {
    return number.toFixed(3);
  }

  return number.toFixed(5);
}

/* ================= STYLES ================= */

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#05070B',
  },

  flex: {
    flex: 1,
  },

  content: {
    padding: 18,
    paddingBottom: 100,
  },

  screen: {
    flex: 1,
    padding: 18,
    paddingBottom: 85,
  },

  chartScreen: {
    flex: 1,
    backgroundColor: '#05070B',
    paddingBottom: 80,
  },

  header: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 25,
  },

  logo: {
    color: '#FFFFFF',
    fontSize: 34,
    fontWeight: '900',
    letterSpacing: 2,
  },

  blue: {
    color: '#4C8DFF',
  },

  tagline: {
    color: '#687386',
    fontSize: 9,
    letterSpacing: 2,
  },

  iconButton: {
    width: 44,
    height: 44,
    borderRadius: 22,
    backgroundColor: '#111722',
    justifyContent: 'center',
    alignItems: 'center',
  },

  accountCard: {
    backgroundColor: '#101827',
    borderRadius: 18,
    padding: 20,
    marginBottom: 25,
    borderWidth: 1,
    borderColor: '#1C3558',
  },

  balance: {
    color: '#FFFFFF',
    fontSize: 30,
    fontWeight: '800',
    marginTop: 5,
    marginBottom: 20,
  },

  rowBetween: {
    flexDirection: 'row',
    justifyContent: 'space-between',
  },

  white: {
    color: '#FFFFFF',
    fontSize: 14,
    fontWeight: '700',
  },

  whiteSmall: {
    color: '#FFFFFF',
    fontSize: 11,
    fontWeight: '700',
    marginTop: 3,
  },

  muted: {
    color: '#7A8597',
    fontSize: 12,
  },

  mutedSmall: {
    color: '#667085',
    fontSize: 10,
    marginTop: 3,
  },

  smallLabel: {
    color: '#667085',
    fontSize: 11,
  },

  green: {
    color: '#35D07F',
  },

  red: {
    color: '#FF5C68',
  },

  sectionTitle: {
    color: '#FFFFFF',
    fontSize: 19,
    fontWeight: '800',
    marginBottom: 13,
  },

  quickRow: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    marginBottom: 25,
  },

  quickButton: {
    width: '31%',
    backgroundColor: '#101722',
    borderRadius: 14,
    padding: 16,
    alignItems: 'center',
  },

  quickIcon: {
    fontSize: 23,
    marginBottom: 6,
  },

  quickText: {
    color: '#FFFFFF',
    fontSize: 11,
    fontWeight: '700',
  },

  favoriteRow: {
    backgroundColor: '#0D131D',
    borderRadius: 13,
    padding: 14,
    marginBottom: 8,
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
  },

  aiCard: {
    backgroundColor: '#111A2A',
    borderRadius: 18,
    padding: 18,
    marginTop: 15,
    borderWidth: 1,
    borderColor: '#294F88',
  },

  aiTitle: {
    color: '#FFFFFF',
    fontSize: 17,
    fontWeight: '800',
    marginBottom: 10,
  },

  aiText: {
    color: '#AAB4C4',
    fontSize: 13,
    lineHeight: 21,
    textAlign: 'center',
  },

  aiFeatures: {
    color: '#6FA4FF',
    fontSize: 11,
    fontWeight: '700',
    marginTop: 5,
  },

  primaryButton: {
    backgroundColor: '#3F7BFF',
    paddingVertical: 13,
    paddingHorizontal: 25,
    borderRadius: 10,
    marginTop: 15,
    alignItems: 'center',
  },

  secondaryButton: {
    borderWidth: 1,
    borderColor: '#273449',
    paddingVertical: 12,
    paddingHorizontal: 25,
    borderRadius: 10,
    marginTop: 15,
  },

  buttonText: {
    color: '#FFFFFF',
    fontWeight: '800',
    fontSize: 12,
  },

  topHeader: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 15,
  },

  screenTitle: {
    color: '#FFFFFF',
    fontSize: 25,
    fontWeight: '800',
  },

  live: {
    color: '#35D07F',
    fontSize: 10,
    fontWeight: '800',
  },

  searchBox: {
    height: 46,
    backgroundColor: '#101722',
    borderRadius: 12,
    paddingHorizontal: 13,
    flexDirection: 'row',
    alignItems: 'center',
    marginBottom: 10,
  },

  searchInput: {
    flex: 1,
    color: '#FFFFFF',
    marginLeft: 8,
  },

  category: {
    backgroundColor: '#101722',
    borderRadius: 20,
    paddingHorizontal: 15,
    paddingVertical: 9,
    marginRight: 7,
  },

  categoryActive: {
    backgroundColor: '#214D9A',
  },

  categoryText: {
    color: '#7A8597',
    fontSize: 11,
    fontWeight: '700',
  },

  categoryTextActive: {
    color: '#FFFFFF',
  },

  marketRow: {
    minHeight: 72,
    borderBottomWidth: 1,
    borderBottomColor: '#141B25',
    flexDirection: 'row',
    alignItems: 'center',
  },

  star: {
    width: 30,
  },

  starText: {
    color: '#F5C451',
    fontSize: 21,
  },

  assetInfo: {
    flex: 1,
  },

  change: {
    fontSize: 11,
    fontWeight: '800',
  },

  chartHeader: {
    height: 65,
    paddingHorizontal: 15,
    flexDirection: 'row',
    alignItems: 'center',
  },

  back: {
    color: '#FFFFFF',
    fontSize: 38,
    width: 40,
  },

  chartTitle: {
    color: '#FFFFFF',
    fontSize: 18,
    fontWeight: '800',
  },

  timeframeRow: {
    maxHeight: 43,
    paddingHorizontal: 10,
    borderTopWidth: 1,
    borderBottomWidth: 1,
    borderColor: '#151D28',
  },

  tfButton: {
    paddingHorizontal: 10,
    paddingVertical: 8,
    marginRight: 4,
    borderRadius: 7,
  },

  tfActive: {
    backgroundColor: '#214D9A',
  },

  tfText: {
    color: '#6F7A8C',
    fontSize: 11,
    fontWeight: '700',
  },

  tfTextActive: {
    color: '#FFFFFF',
  },

  chartTools: {
    height: 40,
    flexDirection: 'row',
    alignItems: 'center',
    paddingHorizontal: 12,
  },

  tool: {
    color: '#A5AFBE',
    fontSize: 16,
    marginRight: 20,
  },

  refresh: {
    marginLeft: 'auto',
  },

  center: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    paddingHorizontal: 25,
    paddingBottom: 80,
  },

  chartArea: {
    flexDirection: 'row',
    backgroundColor: '#070A0F',
  },

  priceLabels: {
    width: 52,
    justifyContent: 'space-between',
    paddingVertical: 10,
  },

  priceLabel: {
    color: '#657083',
    fontSize: 8,
    textAlign: 'right',
    paddingRight: 5,
  },

  candleArea: {
    position: 'relative',
    overflow: 'hidden',
  },

  grid: {
    position: 'absolute',
    left: 0,
    right: 0,
    height: 1,
    backgroundColor: '#111722',
  },

  candle: {
    position: 'absolute',
    width: 7,
  },

  wick: {
    position: 'absolute',
    left: 3,
    width: 1,
  },

  body: {
    position: 'absolute',
    left: 1,
    width: 6,
    borderRadius: 1,
  },

  analysisCard: {
    backgroundColor: '#101827',
    borderRadius: 14,
    margin: 12,
    padding: 13,
    borderWidth: 1,
    borderColor: '#294F88',
  },

  analysisGrid: {
    flexDirection: 'row',
    flexWrap: 'wrap',
    justifyContent: 'space-between',
    marginBottom: 8,
  },

  analysisItem: {
    width: '31%',
    backgroundColor: '#0B111A',
    borderRadius: 8,
    padding: 8,
    marginBottom: 7,
  },

  tradeBar: {
    position: 'absolute',
    bottom: 82,
    left: 15,
    right: 15,
    flexDirection: 'row',
  },

  sell: {
    flex: 1,
    backgroundColor: '#9D2936',
    paddingVertical: 13,
    borderRadius: 10,
    alignItems: 'center',
    marginRight: 5,
  },

  buy: {
    flex: 1,
    backgroundColor: '#187A50',
    paddingVertical: 13,
    borderRadius: 10,
    alignItems: 'center',
    marginLeft: 5,
  },

  retry: {
    backgroundColor: '#214D9A',
    paddingHorizontal: 25,
    paddingVertical: 11,
    borderRadius: 10,
    marginTop: 15,
  },

  bigIcon: {
    fontSize: 55,
    marginBottom: 15,
  },

  bigTitle: {
    color: '#FFFFFF',
    fontSize: 28,
    fontWeight: '800',
    marginBottom: 15,
  },

  bottomNav: {
    position: 'absolute',
    left: 0,
    right: 0,
    bottom: 0,
    height: 72,
    backgroundColor: '#090D13',
    borderTopWidth: 1,
    borderTopColor: '#1B2430',
    flexDirection: 'row',
    justifyContent: 'space-around',
    alignItems: 'center',
  },

  navItem: {
    alignItems: 'center',
    width: 65,
  },

  navIcon: {
    color: '#687386',
    fontSize: 22,
  },

  navText: {
    color: '#687386',
    fontSize: 9,
    marginTop: 3,
  },

  navActive: {
    color: '#4C8DFF',
  },
});
