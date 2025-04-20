In both monolithic and microservices architectures, services requests are typically executed either synchronously or asynchronously.

In a synchronous request, a service request will remain in a waiting state until the transaction is completed, at which point the response is returned to the client. Therefore, upon making a request, we must await a successful response in order to proceed with fulfilling the request. Failure to do so would negatively impact the customer experience while using the product.

In order to circumvent synchronous communications, one can depend on design patterns such as the [[../Data/CQRS]] Pattern and [[../Data/SAGA]] Pattern, which will be further elaborated upon in forthcoming articles.

On the contrary, in an asynchronous request, a service request is executed intermittently by first transmitting a status update concerning the availability of the service, and subsequently processing the remaining request.