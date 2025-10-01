# 🏷️ Dynamic League Name Implementation

## ✨ **Feature Overview**

The dashboard now automatically fetches and displays the user's actual mini league name from the FPL API, replacing the generic "FPL Dashboard" title with their real league name (e.g., "PepRoulette™", "Fantasy Football League").

## 🔧 **How It Works**

### 1. **Template Behavior**
- Shows generic "FPL Dashboard" when no config is set up
- Safe fallback for new users

### 2. **User Instance Behavior**
- Automatically fetches league name from FPL API using `CLASSIC_LEAGUE_ID`
- Displays actual league name in the dashboard header
- Cached for 1 hour to improve performance

### 3. **Implementation Details**

```python
@st.cache_data(ttl=3600)  # Cache for 1 hour
def get_league_name():
    """Fetch the actual league name from FPL API."""
    try:
        import requests
        from config import CLASSIC_LEAGUE_ID
        
        # Fetch league information from FPL API
        league_url = f"https://fantasy.premierleague.com/api/leagues-classic/{CLASSIC_LEAGUE_ID}/standings/"
        response = requests.get(league_url, timeout=10)
        
        if response.status_code == 200:
            data = response.json()
            league_name = data.get('league', {}).get('name', 'FPL Dashboard')
            return league_name
        else:
            return "FPL Dashboard"
    except ImportError:
        # config.py doesn't exist yet - user hasn't set up
        return "FPL Dashboard"
    except Exception as e:
        print(f"Could not fetch league name: {e}")
        return "FPL Dashboard"
```

## 🎯 **User Experience**

### **For Template Users (No Setup)**
- Dashboard shows: "🏆 FPL Dashboard 🏆"
- Generic, safe placeholder

### **For Configured Users**
- Dashboard shows: "🏆 PepRoulette™ 🏆" (or their actual league name)
- Personalized experience
- No manual configuration needed

## 🛡️ **Security & Privacy**

- ✅ **No private data in template** - Generic fallback
- ✅ **API call only when configured** - Respects user setup
- ✅ **Graceful error handling** - Falls back to generic name
- ✅ **Cached for performance** - Reduces API calls

## 🚀 **Benefits**

1. **Personalized Experience**: Users see their actual league name
2. **Zero Configuration**: Works automatically once `CLASSIC_LEAGUE_ID` is set
3. **Performance Optimized**: 1-hour caching reduces API calls
4. **Template Safe**: Generic fallback for unconfigured instances
5. **Error Resilient**: Graceful handling of API failures

## 📋 **Technical Implementation**

### **Code Changes Made:**
1. **Added `get_league_name()` function** - Fetches league name from FPL API
2. **Updated dashboard header** - Dynamic title display
3. **Added error handling** - Graceful fallbacks
4. **Implemented caching** - Performance optimization
5. **Updated documentation** - Clear user instructions

### **API Endpoint Used:**
```
GET https://fantasy.premierleague.com/api/leagues-classic/{CLASSIC_LEAGUE_ID}/standings/
```

### **Response Structure:**
```json
{
  "league": {
    "name": "PepRoulette™",
    "id": 123456,
    ...
  },
  ...
}
```

## 🎉 **Result**

Users now get a **personalized dashboard experience** that automatically displays their actual mini league name, while the template remains completely generic and safe for public distribution!

---

**Perfect balance between template safety and user personalization! 🏆**
