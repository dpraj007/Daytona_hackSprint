# Real Seller APIs for Mimicking Seller Behavior

This document compiles real-world seller APIs that can be integrated into the MAMS (Multi-Agent Marketplace Simulator) to make seller behavior more realistic and data-driven.

## Table of Contents

1. [eBay Seller APIs](#ebay-seller-apis)
2. [Etsy Seller APIs](#etsy-seller-apis)
3. [E-commerce Data APIs](#e-commerce-data-apis)
4. [Integration Recommendations](#integration-recommendations)

---

## eBay Seller APIs

### 1. **Negotiation API** ⭐ **HIGHLY RECOMMENDED**
**URL:** https://developer.ebay.com/api-docs/sell/negotiation/overview.html  
**Version:** v1.1.2

**Key Features:**
- **findEligibleItems**: Retrieve listings that buyers have shown interest in (watchlist, cart abandonment)
- **sendOfferToInterestedBuyers**: Send discount offers to interested buyers
- Supports percentage discounts or fixed price reductions
- Offer duration: 2-4 days depending on marketplace

**Use Cases for MAMS:**
- Mimic real seller offer behavior based on buyer interest signals
- Implement realistic offer timing and expiration
- Track buyer engagement patterns (watchlist, cart abandonment)

**API Endpoints:**
- `GET /sell/negotiation/v1/offer/find_eligible_items` - Find items eligible for offers
- `POST /sell/negotiation/v1/offer/send_offer_to_interested_buyers` - Send offers

**Marketplaces Supported:**
- EBAY_US, EBAY_GB, EBAY_AU, EBAY_CA, EBAY_FR, EBAY_DE, EBAY_IT, EBAY_ES

**Documentation:**
- Overview: https://developer.ebay.com/api-docs/sell/negotiation/overview.html
- API Reference: https://developer.ebay.com/api-docs/sell/negotiation/resources/methods
- OpenAPI Spec: https://developer.ebay.com/api-docs/master/sell/negotiation/openapi/3/sell_negotiation_v1_oas3.json

---

### 2. **Message API**
**URL:** https://developer.ebay.com/api-docs/commerce/message/overview.html

**Key Features:**
- Send messages to buyers
- Retrieve conversations
- Modify conversation status
- Real-time buyer-seller communication

**Use Cases for MAMS:**
- Enhance seller communication patterns
- Track conversation history and response times
- Implement realistic messaging delays and patterns

---

### 3. **Inventory API**
**URL:** https://developer.ebay.com/api-docs/sell/inventory/static/overview.html

**Key Features:**
- Create and manage inventory item records
- Convert inventory items into product offers
- Manage pricing, quantity, and availability
- Bulk inventory operations

**Use Cases for MAMS:**
- Realistic inventory management behavior
- Dynamic pricing based on inventory levels
- Quantity-based negotiation strategies

---

### 4. **Best Offer API (Traditional)**
**URL:** https://developer.ebay.com/Devzone/XML/docs/Reference/eBay/StandardListingIndex.html

**Key Features:**
- Accept, decline, or counter buyer offers
- Manage best offer listings
- Track offer history and statistics

**Use Cases for MAMS:**
- Implement counter-offer strategies
- Track offer acceptance/rejection rates
- Model seller decision-making on offers

---

### 5. **Analytics & Reporting API**
**URL:** https://developer.ebay.com/develop/selling-apps/analytics-and-reporting

**Key Features:**
- Selling metrics and performance data
- Listing traffic reports
- Sales analytics

**Use Cases for MAMS:**
- Data-driven seller behavior based on performance metrics
- Realistic pricing strategies based on historical data
- Inventory decisions based on sales trends

---

## Etsy Seller APIs

### 1. **Shop Listing API**
**URL:** https://developers.etsy.com/documentation/reference/#tag/ShopListing

**Key Features:**
- Create, update, and manage shop listings
- Get listing details and inventory
- Manage pricing and variations
- Update listing status

**Use Cases for MAMS:**
- Realistic listing management behavior
- Price adjustment patterns
- Inventory update strategies

**API Endpoints:**
- `GET /v3/application/shops/{shop_id}/listings` - Get shop listings
- `POST /v3/application/shops/{shop_id}/listings` - Create listing
- `PUT /v3/application/shops/{shop_id}/listings/{listing_id}` - Update listing
- `GET /v3/application/listings/{listing_id}` - Get listing details

**Documentation:**
- Reference: https://developers.etsy.com/documentation/reference/#tag/ShopListing
- Authentication: OAuth 2.0 required

---

### 2. **Shop Receipt API**
**URL:** https://developers.etsy.com/documentation/reference/#tag/ShopReceipt

**Key Features:**
- Retrieve order information
- Update order status
- Manage customer communications

**Use Cases for MAMS:**
- Post-purchase seller behavior
- Order fulfillment patterns
- Customer service interactions

---

## E-commerce Data APIs

### 1. **SellerApp E-commerce Data API**
**URL:** https://www.sellerapp.com/ecommerce-data-api.html

**Key Features:**
- Access to 350+ million product pages across 45 marketplaces
- Product attributes, pricing data, promotional information
- Seller account data and demand trends
- Reviews and ratings data

**Use Cases for MAMS:**
- Real product data for inventory
- Realistic pricing patterns
- Market demand trends
- Competitive pricing strategies

**Data Available:**
- Product details and attributes
- Historical pricing data
- Seller performance metrics
- Customer reviews and ratings
- Demand trends and seasonality

---

### 2. **TagX E-commerce API**
**URL:** https://tagxdata.com/ecommerce-api

**Key Features:**
- Real-time product data collection
- Price monitoring across platforms
- Market research capabilities
- Inventory management data

**Use Cases for MAMS:**
- Real-time price monitoring
- Competitive analysis
- Market research for seller strategies
- Dynamic pricing based on market conditions

---

### 3. **HasData E-Commerce APIs**
**URL:** https://hasdata.com/apis/ecommerce

**Key Features:**
- Competitor tracking
- Price and trend analysis
- Customer behavior data
- Buy box winner identification
- Seller offers and shipping information

**Use Cases for MAMS:**
- Competitive pricing strategies
- Buy box optimization behavior
- Shipping strategy decisions
- Customer behavior-based pricing

**Data Available:**
- Product details and pricing
- Buy box winner data
- Seller offers and promotions
- Shipping information
- Reviews and best seller rankings

---

### 4. **Traject Data Unauthorized Seller Monitoring API**
**URL:** https://trajectdata.com/unauthorized-seller-monitoring-scraping-api/

**Key Features:**
- Core SKU and marketplace tracking
- Real-time pricing and seller data
- Violation threshold monitoring
- Automated alerts and enforcement

**Use Cases for MAMS:**
- Seller compliance behavior
- Price monitoring and adjustments
- Marketplace policy adherence
- Competitive response strategies

---

## Integration Recommendations

### Priority 1: eBay Negotiation API ⭐
**Why:** Most directly relevant to MAMS negotiation behavior
- Provides real seller offer patterns
- Buyer interest signals (watchlist, cart abandonment)
- Offer timing and expiration logic
- Percentage vs. fixed discount strategies

**Integration Steps:**
1. Register for eBay Developer account
2. Get API credentials (App ID, Cert ID, Dev ID)
3. Implement OAuth 2.0 authentication
4. Create wrapper service for Negotiation API calls
5. Use real data to inform seller agent prompts

**Code Location:** `backend/app/services/ebay_integration.py` (to be created)

---

### Priority 2: E-commerce Data APIs
**Why:** Provide realistic product and pricing data
- Real product catalogs
- Historical pricing patterns
- Market demand trends
- Competitive pricing data

**Integration Steps:**
1. Choose one or more data providers (SellerApp, TagX, HasData)
2. Set up API accounts and authentication
3. Create data ingestion service
4. Cache product/pricing data locally
5. Use data to populate seller inventory and inform pricing strategies

**Code Location:** `backend/app/services/marketplace_data_service.py` (to be created)

---

### Priority 3: eBay Message API
**Why:** Enhance communication realism
- Real messaging patterns
- Response time data
- Conversation flow patterns

**Integration Steps:**
1. Integrate Message API alongside Negotiation API
2. Analyze real conversation patterns
3. Use patterns to inform seller agent response timing and style

---

## Implementation Architecture

### Proposed Service Structure

```
backend/app/services/
├── ebay_integration/
│   ├── __init__.py
│   ├── negotiation_client.py      # eBay Negotiation API client
│   ├── message_client.py          # eBay Message API client
│   └── inventory_client.py        # eBay Inventory API client
├── marketplace_data/
│   ├── __init__.py
│   ├── sellerapp_client.py        # SellerApp API client
│   ├── tagx_client.py             # TagX API client
│   └── data_cache.py              # Local data caching
└── seller_behavior_engine.py      # Orchestrates real API data for seller agents
```

### Integration with Existing Seller Agent

The existing `SellerAgent` class (`backend/app/agents/seller_agent.py`) can be enhanced to:

1. **Use Real Pricing Data:**
   - Query marketplace data APIs for competitive pricing
   - Adjust offers based on market conditions
   - Use historical pricing trends

2. **Implement Real Offer Patterns:**
   - Use eBay Negotiation API patterns for offer timing
   - Model buyer interest signals (watchlist, cart abandonment)
   - Implement realistic offer expiration logic

3. **Enhance Communication:**
   - Use real message patterns from eBay Message API
   - Implement realistic response delays
   - Model conversation flow patterns

### Example Integration Code

```python
# backend/app/services/ebay_integration/negotiation_client.py
from typing import List, Dict
import httpx
from ..core.config import settings

class eBayNegotiationClient:
    """Client for eBay Negotiation API."""
    
    def __init__(self, access_token: str):
        self.access_token = access_token
        self.base_url = "https://api.ebay.com/sell/negotiation/v1"
        self.headers = {
            "Authorization": f"Bearer {access_token}",
            "Content-Type": "application/json"
        }
    
    async def find_eligible_items(
        self, 
        limit: int = 20,
        offset: int = 0
    ) -> List[Dict]:
        """
        Find items eligible for seller-initiated offers.
        Mimics real seller behavior of identifying interested buyers.
        """
        async with httpx.AsyncClient() as client:
            response = await client.get(
                f"{self.base_url}/offer/find_eligible_items",
                headers=self.headers,
                params={"limit": limit, "offset": offset}
            )
            response.raise_for_status()
            return response.json().get("eligibleItems", [])
    
    async def send_offer(
        self,
        listing_id: str,
        offer_discount_percentage: float = None,
        offer_discount_amount: float = None,
        offer_duration: str = "P2D"  # ISO 8601 duration
    ) -> Dict:
        """
        Send offer to interested buyers.
        Can be used to model real seller offer behavior.
        """
        payload = {
            "listingId": listing_id,
            "offerDuration": offer_duration
        }
        
        if offer_discount_percentage:
            payload["offerDiscountPercentage"] = offer_discount_percentage
        elif offer_discount_amount:
            payload["offerDiscountAmount"] = offer_discount_amount
        
        async with httpx.AsyncClient() as client:
            response = await client.post(
                f"{self.base_url}/offer/send_offer_to_interested_buyers",
                headers=self.headers,
                json=payload
            )
            response.raise_for_status()
            return response.json()
```

---

## API Authentication Requirements

### eBay APIs
- **OAuth 2.0** required
- Register at: https://developer.ebay.com/join
- Need: App ID, Cert ID, Dev ID
- Scopes: `https://api.ebay.com/oauth/api_scope/sell.negotiation`

### Etsy APIs
- **OAuth 2.0** required
- Register at: https://www.etsy.com/developers/register
- Need: API Key, Shared Secret
- Scopes: `listings_r`, `listings_w`, `shops_rw`

### E-commerce Data APIs
- **API Key** authentication (varies by provider)
- Registration required on respective platforms
- Rate limits apply (check provider documentation)

---

## Rate Limits & Costs

### eBay APIs
- **Rate Limits:** Varies by API and account type
- **Cost:** Free for basic usage, paid tiers available
- **Documentation:** https://developer.ebay.com/develop/get-started/api-call-limits

### E-commerce Data APIs
- **Rate Limits:** Varies by provider and plan
- **Cost:** Typically paid (subscription-based)
- **Free Tiers:** Limited (check individual providers)

---

## Next Steps

1. **Evaluate APIs:** Review each API's documentation and pricing
2. **Choose Primary APIs:** Select 1-2 APIs to start with (recommend eBay Negotiation API)
3. **Set Up Accounts:** Register and obtain API credentials
4. **Create Integration Services:** Build client wrappers for selected APIs
5. **Enhance Seller Agent:** Integrate real API data into seller behavior logic
6. **Test & Iterate:** Test with real data and refine seller behavior patterns

---

## Additional Resources

- **eBay Developer Portal:** https://developer.ebay.com/
- **Etsy Developer Portal:** https://developers.etsy.com/
- **eBay Negotiation API Guide:** https://developer.ebay.com/api-docs/sell/static/marketing/offers-to-buyers.html
- **Etsy API Documentation:** https://developers.etsy.com/documentation/

---

**Last Updated:** 2025-11-18  
**Status:** Research Complete - Ready for Implementation

