import React, { useState, useMemo } from 'react';
import { Phone, TrendingUp, Megaphone, Search, ChevronRight, FileText, Clock, Shield, ArrowRight, LayoutGrid } from 'lucide-react';

/**
 * BRAND CONFIGURATION
 * Maroon/Red accent color.
 */
const BRAND_COLORS = {
  primary: 'bg-slate-900', 
  primaryText: 'text-slate-900',
  accent: 'rose', 
  accentHex: '#be123c', 
  bg: 'bg-slate-50'
};

/**
 * CONTENT DATABASE
 */
const INITIAL_ARTICLES = [
  // --- POLICY & EXPECTATIONS ---
  {
    id: 'policy-1',
    category: 'Policy',
    title: 'DNC Compliance: Your Options & Liabilities',
    excerpt: 'A clear breakdown of Land Caller’s policies regarding Do Not Call registries, scrubbing, and liability waivers.',
    readTime: '4 min read',
    content: `
      <h3 class="text-xl font-bold text-slate-800 mb-4">Understanding the Landscape</h3>
      <p class="mb-4">At Land Caller, we prioritize protecting our clients and our callers. The regulatory environment regarding the Telephone Consumer Protection Act (TCPA) and National Do Not Call (DNC) Registry is strict. As a client, you have specific choices regarding how your data is handled.</p>
      
      <div class="bg-blue-50 border-l-4 border-blue-600 p-4 mb-6 rounded-r-lg">
        <p class="font-semibold text-blue-900">Key Takeaway:</p>
        <p class="text-blue-800">You are responsible for the data you provide. Whether you use our scrubbing services or a third party, the ultimate liability for calling numbers on the DNC list rests with the client.</p>
      </div>

      <h3 class="text-xl font-bold text-slate-800 mb-4">Option 1: Land Caller Scrubbing</h3>
      <p class="mb-4">We strongly encourage all clients to allow us to filter calling lists against federal and state DNC registries. This is the safest route to minimize legal exposure.</p>

      <h3 class="text-xl font-bold text-slate-800 mb-4">Option 2: Opting Out (The Risks)</h3>
      <p class="mb-4">Some investors choose to call DNC numbers, arguing that offers to purchase property do not constitute "telemarketing." <strong>Warning:</strong> Recent court rulings have challenged this interpretation. If you choose to Opt-Out of scrubbing:</p>
      <ul class="list-disc pl-6 mb-4 space-y-2 marker:text-rose-600">
        <li>You must sign our <strong>Indemnification Agreement</strong>.</li>
        <li>You explicitly acknowledge that dialing DNC numbers may expose you to fines or enforcement actions.</li>
        <li>You release Land Caller from all liability associated with these calls.</li>
      </ul>

      <h3 class="text-xl font-bold text-slate-800 mb-4">Third-Party Scrubbing</h3>
      <p class="mb-4">If you bring your own pre-scrubbed data, you must warrant that the scrubbing was performed within the last 31 days and that you possess a valid SAN (Subscription Account Number) with the FTC. Land Caller assumes no responsibility for the accuracy of third-party scrubs.</p>
    `
  },
  {
    id: 'policy-2',
    category: 'Policy',
    title: 'What to Expect: The First 30 Days',
    excerpt: 'Realistic timelines for campaign ramp-up, caller training, and lead flow consistency.',
    readTime: '3 min read',
    content: `
      <h3 class="text-xl font-bold text-slate-800 mb-4">The Ramp-Up Phase</h3>
      <p class="mb-4">Cold calling is a momentum game, not a light switch. When you launch a new campaign with Land Caller, here is the realistic timeline you should expect.</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">Week 1: Calibration</h4>
      <p class="mb-4">Our callers are learning your specific market nuances. We test data quality and connection rates. Lead flow may be slower as we filter out dead numbers and wrong contacts.</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">Weeks 2-3: The Rhythm</h4>
      <p class="mb-4">Callers begin to build a pipeline. You will see an increase in "callbacks"—prospects who didn't answer initially but are returning calls. This is where follow-up becomes critical.</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">Week 4+: Optimization</h4>
      <p class="mb-4">By this stage, we have enough data to adjust dialing times and approach. We know which zip codes are responding best and can pivot strategy accordingly.</p>

      <div class="bg-amber-50 border-l-4 border-amber-500 p-4 mt-6 rounded-r-lg">
        <h4 class="font-bold text-amber-900">Why Lead Flow Fluctuates</h4>
        <p class="text-amber-800">It is normal for lead flow to vary week-to-week. Factors include list exhaustion (calling the same people too often), holidays, and even weather events in the target market.</p>
      </div>
    `
  },
  {
    id: 'policy-3',
    category: 'Policy',
    title: 'Our Tech Stack: Why We Built Our Own Dialer',
    excerpt: 'We don’t rely on off-the-shelf software. Learn how our custom Convoso integration drives higher contact rates for land investors.',
    readTime: '3 min read',
    content: `
      <h3 class="text-xl font-bold text-slate-800 mb-4">Powered by Convoso, Perfected by Land Caller</h3>
      <p class="mb-4">In the high-volume world of cold calling, technology is the differentiator between a "Busy Signal" and a "Hello." That is why we didn't just pick a dialer off the shelf—we built our own dialing infrastructure on top of the industry-leading <strong>Convoso</strong> platform.</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">Why Customization Matters</h4>
      <p class="mb-4">Generic dialers are built for everyone—insurance agents, solar sales, and debt collectors. But land investing is unique. We customized our Convoso instance specifically for our niche needs:</p>
      <ul class="list-disc pl-6 mb-4 space-y-2 marker:text-rose-600">
        <li><strong>Smart Caller ID Rotation:</strong> To prevent your numbers from being flagged as "Spam Likely," our system rotates numbers intelligently based on carrier algorithms.</li>
        <li><strong>Niche-Specific Logic:</strong> We configured the dialing cadence to match the behavior of property owners, not consumers.</li>
      </ul>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">The Result: Higher Contact Rates</h4>
      <p class="mb-4">Because we control the backend, we can adapt instantly. If a major carrier updates their filtering rules, we update our strategy. This ensures that your leads actually pick up the phone, giving our agents more "at-bats" to uncover deals for you.</p>
    `
  },

  // --- LEAD FOLLOW-UP & CLOSING (Formerly Cold Calling) ---
  {
    id: 'cc-1',
    category: 'Cold Calling', 
    title: 'Do You Provide Follow-Up Scripts?',
    excerpt: 'We don’t offer a one-size-fits-all script because every investor is unique, but here are our best practices for structuring your call-backs.',
    readTime: '3 min read',
    content: `
      <h3 class="text-xl font-bold text-slate-800 mb-4">Why We Don't Use a Standard Script</h3>
      <p class="mb-4">A common question we get is, <em>"Can you send me a script for calling back my leads?"</em></p>
      <p class="mb-4">We generally do not provide a fixed script for call-backs. We have such a wide range of clients at Land Caller—from seasoned developers to first-time flippers—and each has their own approach and strategy. A script that works for a low-ball volume offer strategy might fail for a relationship-based negotiator.</p>
      
      <div class="bg-rose-50 border-l-4 border-rose-600 p-4 mb-6 rounded-r-lg">
        <p class="font-semibold text-rose-900">Our Recommendation:</p>
        <p class="text-rose-800">Instead of reading from a page, focus on these three core pillars of a successful follow-up call.</p>
      </div>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">1. Research First, Dial Second</h4>
      <p class="mb-4">Do some basic research on the property prior to calling back and have it in front of you to reference. Knowing at least a range of the price you want to be in can help you tease out the seller's number if they don't offer it up.</p>
      <p class="mb-4"><strong>Why it works:</strong> Speaking about details on the property that you see from the map (e.g., "I see you have road access on the north side") relays a sense of professionalism and seriousness, which leads to higher conversions.</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">2. The Art of Open-Ended Questions</h4>
      <p class="mb-4">Avoid Yes/No questions. Your goal is to get them talking about their life, not just the dirt.</p>
      <ul class="list-disc pl-6 mb-4 space-y-2 marker:text-rose-600 italic text-slate-600">
        <li>"Do you have plans for the property?"</li>
        <li>"Have you been to the lot recently? What's your favorite aspect of it?"</li>
      </ul>
      <p class="mb-4">This builds rapport and arms you with valuable information (pain points, timeline) to use during negotiations.</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">3. Feel Out the Seller's Style</h4>
      <p class="mb-4">Some sellers want to get right down to business and discuss price in the first 30 seconds. Others want to chat about the weather and will get turned off if you drive straight to the numbers.</p>
      <p class="mb-4">This takes time to develop, but approaching each lead in a manner best suited to their personality—mirroring their energy—will pay off significantly more than a rigid script.</p>
    `
  },
  {
    id: 'cc-2',
    category: 'Cold Calling', 
    title: 'Master Class: The Follow-Up Call',
    excerpt: 'Your Land Caller agent has opened the door. Now it is your turn to close. Here is how to handle the "Not Now" leads.',
    readTime: '6 min read',
    content: `
      <h3 class="text-xl font-bold text-slate-800 mb-4">Bridging the Gap</h3>
      <p class="mb-4">Land Caller has done the heavy lifting: we've dialed the numbers, filtered out the wrong contacts, and identified property owners willing to talk. Now, you are calling back a "Warm Lead."</p>
      <p class="mb-4">However, "Warm" doesn't always mean "Ready." Here is how to handle the leads we send you who are still on the fence.</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">1. The "Soft Opening" Technique</h4>
      <p class="mb-4">Do not start like a cold caller. Start like a partner. Reference the previous conversation immediately to build trust.</p>
      <p class="bg-slate-100 p-4 rounded-md font-mono text-sm text-slate-700 border border-slate-200">"Hi [Name], this is [Your Name]. I’m following up on a conversation you had with my assistant/team member recently regarding your property in [County]. I'm looking at their notes here..."</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">2. Handling the "Not Ready" Objection</h4>
      <p class="mb-4">If our notes say they are interested but they tell you "I'm not looking to sell right now," don't panic. They are often just guarded.</p>
      <p class="mb-2"><strong>The Pivot Script:</strong></p>
      <p class="bg-slate-100 p-4 rounded-md font-mono text-sm text-slate-700 border border-slate-200">"I completely understand. My team mentioned you might be holding off for now. But out of curiosity—since I'm the one who actually writes the checks—if I could present a cash offer that made sense today, would you at least be open to seeing it?"</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">3. The Price Anchor</h4>
      <p class="mb-4">When a seller says "Make me an offer," they are testing you. Since you are the decision maker (unlike the cold caller), you can get specific.</p>
      <p class="bg-slate-100 p-4 rounded-md font-mono text-sm text-slate-700 border border-slate-200">"I'd love to make an offer. Based on what my team told me about the condition, I’m seeing similar lots go for [Range]. Does that sound like it’s in the ballpark of what you were hoping for?"</p>
    `
  },
  {
    id: 'cc-3',
    category: 'Cold Calling',
    title: 'Analyzing Your Lead Quality',
    excerpt: 'Why some batches of leads we send you are hotter than others, and how to read the "Caller Notes" effectively.',
    readTime: '4 min read',
    content: `
      <h3 class="text-xl font-bold text-slate-800 mb-4">Interpreting the Handoff</h3>
      <p class="mb-4">When you receive a batch of leads from Land Caller, you might notice variances in motivation. Understanding <em>why</em> helps you prioritize who to call back first.</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">The "Tired Landlord" vs. The "Curious Neighbor"</h4>
      <p class="mb-4">Our callers categorize leads based on sentiment. 
      <br><strong>High Priority:</strong> Leads who asked specific questions about price or timeline.
      <br><strong>Medium Priority:</strong> Leads who simply confirmed ownership and didn't hang up. These require more "warming up" on your follow-up call.</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">List Exhaustion Factors</h4>
      <p class="mb-4">If you provided us with a generic "Tax Delinquent" list, your follow-up calls might meet more resistance because 50 other investors are also calling them. <strong>Pro Tip:</strong> Your conversion rate on our leads will skyrocket if you provide us with niche lists (e.g., specific zoning or inherited properties) where the competition is lower.</p>
    `
  },

  // --- INDUSTRY EDUCATION ---
  {
    id: 'ind-1',
    category: 'Industry',
    title: 'Recession Resilience: Why Land Wins',
    excerpt: 'Market analysis on why raw land remains a stable asset class during economic downturns.',
    readTime: '5 min read',
    content: `
      <h3 class="text-xl font-bold text-slate-800 mb-4">The Asset No One is Making More Of</h3>
      <p class="mb-4">When the housing market cools, house flippers panic. Margins shrink as renovation costs stay high and sales prices drop. Land investors, however, operate in a different reality.</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">Zero Holding Costs (Almost)</h4>
      <p class="mb-4">Unlike a rental property with leaky roofs or a flip with a high-interest hard money loan, vacant land costs pennies to hold. In a recession, you can sit on inventory without bleeding cash.</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">Cash is King</h4>
      <p class="mb-4">During recessions, banks tighten lending. Since most land deals are cash transactions (both buying and selling via owner finance), the land market is less sensitive to interest rate hikes than the traditional housing market.</p>
    `
  },

  // --- MARKETING CONTENT ---
  {
    id: 'mkt-1',
    category: 'Marketing',
    title: 'SEO for Land Investors: A Primer',
    excerpt: 'How to structure your buying website to capture organic seller leads.',
    readTime: '7 min read',
    content: `
      <h3 class="text-xl font-bold text-slate-800 mb-4">Stop Renting Your Traffic</h3>
      <p class="mb-4">Cold calling is outbound. SEO is inbound. You need both. When we call a seller, the first thing they do is Google your company name. What do they see?</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">Credibility Content</h4>
      <p class="mb-4">Your site needs an "About Us" page that shows real faces. Land scams are common; proving you are a real human in the US is your biggest marketing win.</p>

      <h4 class="font-bold text-slate-700 mt-6 mb-2">Keywords to Target</h4>
      <ul class="list-disc pl-6 mb-4 space-y-1 marker:text-rose-600">
        <li>"Sell land fast in [County Name]"</li>
        <li>"We buy land [State]"</li>
        <li>"How to sell inherited land"</li>
      </ul>
      <p class="mb-4">Create blog posts answering these specific questions to capture high-intent sellers who are actively looking for a solution.</p>
    `
  }
];

const CATEGORIES = [
  { id: 'Policy', label: 'Policy & Expectations', icon: Shield, color: 'bg-rose-50 text-rose-700 border-rose-100' },
  { id: 'Cold Calling', label: 'Lead Follow-Up & Closing', icon: Phone, color: 'bg-blue-50 text-blue-700 border-blue-100' },
  { id: 'Industry', label: 'Industry Education', icon: TrendingUp, color: 'bg-slate-100 text-slate-700 border-slate-200' },
  { id: 'Marketing', label: 'Marketing Resources', icon: Megaphone, color: 'bg-indigo-50 text-indigo-700 border-indigo-100' },
];

export default function App() {
  const [activeTab, setActiveTab] = useState('Home');
  const [selectedArticle, setSelectedArticle] = useState(null);
  const [searchQuery, setSearchQuery] = useState('');

  // Background Image Style
  const bgImageStyle = {
    backgroundImage: 'url("https://images.unsplash.com/photo-1500382017468-9049fed747ef?ixlib=rb-4.0.3&auto=format&fit=crop&w=2500&q=80")',
    backgroundSize: 'cover',
    backgroundPosition: 'center',
    backgroundAttachment: 'fixed',
  };

  const filteredArticles = useMemo(() => {
    let filtered = INITIAL_ARTICLES;
    
    if (activeTab !== 'Home') {
      filtered = filtered.filter(article => article.category === activeTab);
    }

    if (searchQuery) {
      filtered = filtered.filter(article => 
        article.title.toLowerCase().includes(searchQuery.toLowerCase()) || 
        article.excerpt.toLowerCase().includes(searchQuery.toLowerCase())
      );
    }
    
    return filtered;
  }, [activeTab, searchQuery]);

  const handleArticleClick = (article) => {
    setSelectedArticle(article);
    window.scrollTo({ top: 0, behavior: 'smooth' });
  };

  const handleBack = () => {
    setSelectedArticle(null);
  };

  return (
    <div className={`min-h-screen ${BRAND_COLORS.bg} font-sans text-slate-900`}>
      {/* Background Overlay */}
      <div className="fixed inset-0 z-0 pointer-events-none" style={bgImageStyle}></div>
      <div className="fixed inset-0 z-0 bg-slate-50/90 pointer-events-none backdrop-blur-[2px]"></div>

      {/* HEADER */}
      <header className={`${BRAND_COLORS.primary} shadow-md sticky top-0 z-20`}>
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-18 flex items-center justify-between py-4">
          
          {/* Logo Section */}
          <div className="flex items-center space-x-3 cursor-pointer group" onClick={() => {setActiveTab('Home'); setSelectedArticle(null);}}>
            <div className={`w-10 h-10 bg-${BRAND_COLORS.accent}-600 rounded-lg flex items-center justify-center shadow-lg transform group-hover:scale-105 transition-all duration-300`}>
              <Phone className="w-6 h-6 text-white" />
            </div>
            <div className="flex flex-col">
              <span className="text-xl font-bold text-white tracking-tight leading-none">
                Land Caller
              </span>
              <span className={`text-xs font-medium text-${BRAND_COLORS.accent}-400 uppercase tracking-widest`}>
                Knowledge Base
              </span>
            </div>
          </div>
          
          {/* Navigation */}
          <div className="hidden md:flex items-center space-x-2 bg-slate-800/50 rounded-lg p-1 border border-slate-700">
            {['Home', ...CATEGORIES.map(c => c.id)].map((tab) => (
              <button
                key={tab}
                onClick={() => { setActiveTab(tab); setSelectedArticle(null); }}
                className={`px-4 py-2 rounded-md text-sm font-medium transition-all duration-200 ${
                  activeTab === tab 
                    ? `bg-${BRAND_COLORS.accent}-600 text-white shadow-md` 
                    : 'text-slate-300 hover:text-white hover:bg-slate-700'
                }`}
              >
                {tab === 'Home' ? 'Dashboard' : tab}
              </button>
            ))}
          </div>
        </div>
      </header>

      <main className="relative z-10 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10">
        
        {/* VIEW: SINGLE ARTICLE */}
        {selectedArticle ? (
          <div className="max-w-4xl mx-auto animate-in fade-in slide-in-from-bottom-4 duration-500">
            <button 
              onClick={handleBack}
              className={`group flex items-center text-sm font-medium text-slate-600 hover:text-${BRAND_COLORS.accent}-700 mb-6 transition-colors bg-white/80 backdrop-blur px-4 py-2 rounded-full shadow-sm w-fit`}
            >
              <div className={`bg-white border border-slate-200 p-1 rounded-full mr-2 group-hover:border-${BRAND_COLORS.accent}-200`}>
                <ChevronRight className="w-4 h-4 rotate-180" />
              </div>
              Back to {activeTab === 'Home' ? 'Dashboard' : activeTab}
            </button>

            <article className="bg-white rounded-2xl shadow-xl border border-slate-200 overflow-hidden">
              <div className="p-8 md:p-10 border-b border-slate-100 bg-gradient-to-br from-white to-slate-50">
                <div className="flex items-center gap-3 mb-6">
                  <span className={`px-3 py-1 rounded-full text-xs font-bold uppercase tracking-wider border ${
                    CATEGORIES.find(c => c.id === selectedArticle.category)?.color || 'bg-slate-100 text-slate-600'
                  }`}>
                    {selectedArticle.category === 'Cold Calling' ? 'Lead Follow-Up & Closing' : selectedArticle.category}
                  </span>
                  <span className="flex items-center text-xs font-medium text-slate-400">
                    <Clock className="w-3.5 h-3.5 mr-1.5" /> {selectedArticle.readTime}
                  </span>
                </div>
                <h1 className="text-3xl md:text-4xl font-extrabold text-slate-900 mb-6 leading-tight tracking-tight">{selectedArticle.title}</h1>
                <p className="text-xl text-slate-600 leading-relaxed font-light">{selectedArticle.excerpt}</p>
              </div>
              
              <div 
                className="p-8 md:p-12 prose prose-slate max-w-none prose-headings:font-bold prose-headings:text-slate-800 prose-p:text-slate-600 prose-li:text-slate-600 prose-strong:text-slate-900 prose-a:text-rose-600"
                dangerouslySetInnerHTML={{ __html: selectedArticle.content }}
              />
            </article>

            {/* Next Steps / CTA */}
            <div className="mt-8 bg-slate-900 rounded-2xl p-8 text-center shadow-2xl relative overflow-hidden">
              <div className={`absolute top-0 right-0 w-64 h-64 bg-${BRAND_COLORS.accent}-600/20 rounded-full blur-3xl transform translate-x-1/2 -translate-y-1/2`}></div>
              <div className="relative z-10">
                <h4 className="text-xl font-bold text-white mb-2">Need to update your campaign settings?</h4>
                <p className="text-slate-300 mb-6 max-w-lg mx-auto">Contact your dedicated account manager to apply these strategies to your current list today.</p>
                <button className={`bg-${BRAND_COLORS.accent}-600 text-white px-8 py-3 rounded-lg text-sm font-bold hover:bg-${BRAND_COLORS.accent}-500 transition-all shadow-lg hover:shadow-${BRAND_COLORS.accent}-500/25 flex items-center mx-auto`}>
                  Contact Support <ArrowRight className="w-4 h-4 ml-2" />
                </button>
              </div>
            </div>
          </div>
        ) : (
          /* VIEW: DASHBOARD / LIST */
          <div className="animate-in fade-in duration-500">
            {/* Hero Section (Only on Home) */}
            {activeTab === 'Home' && !searchQuery && (
              <div className="mb-12">
                <div className="bg-white/95 backdrop-blur rounded-2xl p-8 border border-slate-200 shadow-md flex flex-col md:flex-row items-start md:items-center justify-between gap-6 relative overflow-hidden">
                  <div className={`absolute left-0 top-0 h-full w-1.5 bg-${BRAND_COLORS.accent}-600`}></div>
                  <div className="relative z-10 max-w-2xl">
                    <h1 className="text-3xl font-bold text-slate-900 mb-2">Welcome back, Partner</h1>
                    <p className="text-lg text-slate-500">
                      Stay compliant, stay informed, and close more deals. Explore the latest from the Land Caller team.
                    </p>
                  </div>
                  <div className="relative z-10">
                    <button className="text-sm font-medium text-slate-500 hover:text-slate-900 underline decoration-slate-300 underline-offset-4">
                      View System Status
                    </button>
                  </div>
                </div>
                
                <div className="mt-8 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
                  {CATEGORIES.map((cat) => (
                    <div 
                      key={cat.id} 
                      onClick={() => setActiveTab(cat.id)}
                      className={`group bg-white/90 backdrop-blur p-6 rounded-xl border border-slate-200 shadow-sm hover:shadow-xl hover:border-${BRAND_COLORS.accent}-200 transition-all cursor-pointer relative overflow-hidden`}
                    >
                      <div className={`absolute top-0 right-0 w-24 h-24 ${cat.color.split(' ')[0]} rounded-bl-full opacity-50 transform translate-x-8 -translate-y-8 group-hover:scale-110 transition-transform duration-500`}></div>
                      
                      <div className={`w-12 h-12 rounded-xl flex items-center justify-center mb-4 ${cat.color} border`}>
                        <cat.icon className="w-6 h-6" />
                      </div>
                      <h3 className="font-bold text-slate-900 text-lg group-hover:text-rose-700 transition-colors">{cat.label}</h3>
                      <div className="flex items-center text-xs text-slate-400 mt-2 font-medium">
                        View Resources <ArrowRight className="w-3 h-3 ml-1 opacity-0 -translate-x-2 group-hover:opacity-100 group-hover:translate-x-0 transition-all" />
                      </div>
                    </div>
                  ))}
                </div>
              </div>
            )}

            {/* Search Bar & Title */}
            <div className="flex flex-col md:flex-row justify-between items-center mb-8 gap-4 bg-white/95 backdrop-blur p-4 rounded-xl border border-slate-200 shadow-sm">
              <div className="flex items-center gap-3">
                <div className={`p-2 bg-${BRAND_COLORS.accent}-100 rounded-lg`}>
                  <LayoutGrid className={`w-5 h-5 text-${BRAND_COLORS.accent}-700`} />
                </div>
                <h2 className="text-xl font-bold text-slate-800">
                  {activeTab === 'Home' ? 'Recent Updates' : `${CATEGORIES.find(c => c.id === activeTab)?.label || activeTab} Articles`}
                </h2>
              </div>
              <div className="relative w-full md:w-96 group">
                <Search className="absolute left-3 top-1/2 transform -translate-y-1/2 w-4 h-4 text-slate-400 group-focus-within:text-rose-500 transition-colors" />
                <input 
                  type="text" 
                  placeholder="Search knowledge base..." 
                  value={searchQuery}
                  onChange={(e) => setSearchQuery(e.target.value)}
                  className="w-full pl-10 pr-4 py-2.5 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:ring-2 focus:ring-rose-500/20 focus:border-rose-500 transition-all focus:bg-white"
                />
              </div>
            </div>

            {/* Article Grid */}
            {filteredArticles.length > 0 ? (
              <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                {filteredArticles.map((article) => (
                  <div 
                    key={article.id}
                    onClick={() => handleArticleClick(article)}
                    className={`group bg-white/95 backdrop-blur rounded-xl p-6 border border-slate-200 shadow-sm hover:shadow-xl hover:border-${BRAND_COLORS.accent}-200 transition-all duration-300 cursor-pointer flex flex-col h-full relative overflow-hidden`}
                  >
                    <div className="flex items-center justify-between mb-4 relative z-10">
                      <span className={`px-2.5 py-1 rounded-md text-xs font-bold uppercase tracking-wider border ${
                        CATEGORIES.find(c => c.id === article.category)?.color || 'bg-slate-100 text-slate-600'
                      }`}>
                        {article.category === 'Cold Calling' ? 'Follow-Up' : article.category}
                      </span>
                      <span className="text-xs font-medium text-slate-400 flex items-center">
                        <FileText className="w-3.5 h-3.5 mr-1.5" /> {article.readTime}
                      </span>
                    </div>
                    
                    <h3 className="text-xl font-bold text-slate-900 mb-3 line-clamp-2 group-hover:text-rose-700 transition-colors relative z-10">{article.title}</h3>
                    <p className="text-slate-500 text-sm mb-6 line-clamp-3 flex-grow leading-relaxed relative z-10">{article.excerpt}</p>
                    
                    <div className="flex items-center text-rose-600 text-sm font-bold mt-auto relative z-10">
                      Read Article 
                      <ChevronRight className="w-4 h-4 ml-1 transform group-hover:translate-x-1 transition-transform" />
                    </div>

                    {/* Hover Effect Background */}
                    <div className="absolute inset-0 bg-gradient-to-br from-transparent to-rose-50/50 opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
                  </div>
                ))}
              </div>
            ) : (
              <div className="text-center py-20 bg-white/95 backdrop-blur rounded-xl border border-dashed border-slate-300 shadow-sm">
                <div className="bg-slate-50 w-16 h-16 rounded-full flex items-center justify-center mx-auto mb-4">
                  <Search className="w-8 h-8 text-slate-400" />
                </div>
                <h3 className="text-lg font-bold text-slate-900 mb-1">No articles found</h3>
                <p className="text-slate-500 text-sm">We couldn't find anything matching "{searchQuery}".</p>
                <button 
                  onClick={() => setSearchQuery('')}
                  className="mt-4 text-rose-600 font-medium text-sm hover:text-rose-700"
                >
                  Clear search
                </button>
              </div>
            )}
          </div>
        )}
      </main>
    </div>
  );
}
