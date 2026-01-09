local AuctionManager = {
    CurrentItem = nil,
    HighestBid = 0,
    HighestBidder = nil,
    TimeRemaining = 0,
    IsAuctionActive = false
}

-- Function to start an auction
function AuctionManager:StartAuction(brainrotItem, duration)
    self.CurrentItem = brainrotItem
    self.HighestBid = brainrotItem.BaseValue or 100
    self.HighestBidder = nil
    self.TimeRemaining = duration
    self.IsAuctionActive = true
    
    print("Auction started for: " .. brainrotItem.Name)
end

-- Function to handle a new bid
function AuctionManager:PlaceBid(player, amount)
    if not self.IsAuctionActive then return false, "No active auction" end
    if amount <= self.HighestBid then return false, "Bid too low" end
    
    -- Update lead
    self.HighestBid = amount
    self.HighestBidder = player
    
    -- "Steal" Mechanic: Reset/Extend timer when a bid is placed
    self.TimeRemaining = math.max(self.TimeRemaining, 5) -- Give others 5 seconds to outbid
    
    return true, "Bid placed!"
end
