Flow

The buyer reserves the item with POST /orders, and the seller accepts with POST /orders/{id}/confirm. The order moves to a new awaiting_payment status, and the address is stored but hidden.
The buyer starts payment with POST /orders/{id}/payment. This returns a UPI intent link and QR payload, collected by the platform rather than paid straight to the seller.
The payment provider’s callback (POST /payments/webhook) marks the payment paid and the order confirmed, and generates the pickup code. For the hackathon, POST /payments/mock/{paymentId}/succeed simulates this locally.
GET /orders/{id}/pickup returns 402 Payment Required until payment has succeeded, so the details can’t be fetched early.
When the seller enters the pickup code at handover, the platform releases the held money to the seller’s new private upiId. If the order is cancelled after payment, the buyer is refunded.

Other changes

paymentMethod is now just upi, replacing the cash-on-pickup options, and orders have a paymentStatus field.
Before payment, the buyer sees only the seller’s display name, area and rating, plus the approximate listing location.
Orders now also expire if the buyer doesn’t pay in time, which releases the item back to active.