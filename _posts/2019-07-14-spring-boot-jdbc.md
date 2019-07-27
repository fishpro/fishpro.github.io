//
//  NAAcceShareView.m
//  NetAcce
//
//  Created by shuai on 2018/8/17.
//  Copyright © 2018年 云镶网络科技公司. All rights reserved.
//

#import "NAAcceShareView.h"

@interface NAAcceShareView()




@end

@implementation NAAcceShareView

- (IBAction)closeBtnClicked:(id)sender
{
    [self removeFromSuperview];
    if (self.cancleBtnBlock) {
        self.cancleBtnBlock();
    }
}
- (instancetype)initWithFrame:(CGRect)frame
{
    self = [super initWithFrame:frame];
    if (!self) {
        return nil;
    }
    self = [[[NSBundle mainBundle] loadNibNamed:@"NAAcceShareView" owner:nil options:nil] lastObject];
    self.frame = CGRectMake(0, 0, DEVICE_WIDTH, DEVICE_HEIGHT);
    
    return self;
}

- (void)awakeFromNib
{
    [super awakeFromNib];
    self.backView.layer.cornerRadius = 5;
    self.backView.layer.masksToBounds = YES;
    
//    [self setNeedsLayout];
//    [self layoutIfNeeded];
    [self bringSubviewToFront:self.backView];
    [self btn:self.confirmBtn BackColoAbleClicked:YES];
    [self btn:self.goWodejiangpin BackColoAbleClicked:YES];
}

- (void)btn:(UIButton*)btn BackColoAbleClicked:(BOOL)ableClicked
{
    CAGradientLayer *gl = [CAGradientLayer layer];
    gl.frame = btn.bounds;
    gl.startPoint = CGPointMake(0.99, 0);
    gl.endPoint = CGPointMake(0, 0.91);
    gl.colors = @[(__bridge id)[UIColor colorWithRed:12/255.0 green:184/255.0 blue:249/255.0 alpha:1.0].CGColor, (__bridge id)[UIColor colorWithRed:134/255.0 green:78/255.0 blue:236/255.0 alpha:1.0].CGColor, (__bridge id)[UIColor colorWithRed:134/255.0 green:78/255.0 blue:236/255.0 alpha:1.0].CGColor];
    gl.locations = @[@(0), @(1.0f), @(1.0f)];
    [btn.layer insertSublayer:gl atIndex:0];
    
    btn.layer.masksToBounds = YES;
    btn.layer.cornerRadius = btn.height/2;
    btn.layer.shadowColor = [UIColor colorWithRed:130/255.0 green:81/255.0 blue:236/255.0 alpha:0.16].CGColor;
    btn.layer.shadowOffset = CGSizeMake(0,0);
    btn.layer.shadowOpacity = 1;
    btn.layer.shadowRadius = 19.5;
    if (ableClicked) {
        btn.alpha = 1;
    }else{
        btn.alpha = 0.46;
    }
}
- (IBAction)confirmBtnClicked:(id)sender
{
    [self removeFromSuperview];
    if (self.confirmBtnBlock) {
        self.confirmBtnBlock();
    }
}


- (void)setConfirmStr:(NSString *)confirmStr
{
    _confirmStr = confirmStr;
    [self.confirmBtn setTitle:confirmStr forState:UIControlStateNormal];
}
- (void)setContentStr:(NSString *)contentStr
{
    _contentStr = contentStr; 
    self.sinViewLabel.text = contentStr;
}
- (void)appearAnimation
{
    [UIView animateWithDuration:0.3 delay:0.0 options:UIViewAnimationOptionCurveEaseOut animations:^{
        self.backgroundColor = RCOLOR(0, 0, 0, 0.3);
    } completion:nil];

//    self.backView.transform = CGAffineTransformMakeScale(0.1, 0.1);
//    [UIView animateWithDuration:0.2 animations:^{
//        self.backView.transform = CGAffineTransformMakeScale(1.1, 1.1);
//    } completion:^(BOOL finished) {
//        [UIView animateWithDuration:0.1 animations:^{
//            self.backView.transform = CGAffineTransformIdentity;
//        }];
//    }];
}
- (void)removeFromSuperview
{
    [UIView animateWithDuration:0.3 delay:0.0 options:UIViewAnimationOptionCurveEaseIn animations:^{
        self.backgroundColor = RCOLOR(0, 0, 0, 0.0);
    } completion:^(BOOL finished) {
        [super removeFromSuperview];
    }];
}
/*
// Only override drawRect: if you perform custom drawing.
// An empty implementation adversely affects performance during animation.
- (void)drawRect:(CGRect)rect {
    // Drawing code
}
*/

@end
