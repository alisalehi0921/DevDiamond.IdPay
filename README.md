# DevDiamond.IdPay
سفارشی سازی درگاه بانک واسط آیدی پی



///////////////////////////////////////////  نمونه کد قابل استفاده
using DevDiamond.IdPay.Models;
using DevDiamond.IdPay.Services;
using Microsoft.AspNetCore.Mvc;
using System.Threading.Tasks;

namespace DevDiamondIdPayTest.Controllers
{
    public class HomeController : Controller
    {
        private readonly IdPay idPay;

        public HomeController(IdPay idPay)
        {
            this.idPay = idPay;
        }

        public async Task<IActionResult> Payment()        
        {
            var result = await idPay.PaymentAsync(
               "APIKEY",
                10000,
                $"{Request.Scheme}://{Request.Host}/Home/PaymentResult",
                "تست",
                "123",
                sandBox: true);
            return Redirect(result.Link);
        }

        public async Task<IActionResult> PaymentResult()
        {
            var result = await idPay.VerifyAsync("90442be2-b394-422e-9c96-aa92a1bd237b",true);
            if (result.Status== PaymentResultStatus.Successed)
            {
                /// موفق
            }
            else if (result.Status != PaymentResultStatus.AlreadyProcessed)
            {
                /// خطا
            }
            return View();
        }
    }
}



//////////////////////// وضعیت ها

        /// <summary>
        /// پرداخت انجام نشده است
        /// </summary>
        PaymentNotMade = 1,
        /// <summary>
        /// پرداخت ناموفق بوده است
        /// </summary>
        PaymentFailed = 2,
        /// <summary>
        /// خطایی رخ داده است 
        /// </summary>
        AnErrorHasOccurred = 3,
        /// <summary>
        /// بلوکه شده
        /// </summary>
        Blocked = 4,
        /// <summary>
        /// برگشت به پرداخت کننده
        /// </summary>
        ReturnToPayer = 5,
        /// <summary>
        /// برگشت خورده سیستمی
        /// </summary>
        SystemReversal = 6,
        /// <summary>
        /// در انتظار تایید پرداخت
        /// </summary>
        AwaitingPaymentConfirmation = 10,
        /// <summary>
        /// پرداخت تایید شده است
        /// </summary>
        Successed = 100,
        /// <summary>
        /// پرداخت قبلا تایید شده است
        /// </summary>
        AlreadyProcessed = 101,
        /// <summary>
        /// به دریافت کننده واریز شد
        /// </summary>
        DepositedToTheRecipient = 200,

